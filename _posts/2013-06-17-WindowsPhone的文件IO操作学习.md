---
layout: post
---

# Windows Phone的文件操作学习 

Windows Phone程序的文件可以放在三个地方：独立存储、安装文件夹和SD卡。Windows Phone为文件操作提供了丰富的API，通过这些API，开发者就可以完成具体的文件IO操作。

## 独立存储
Windows Phone为程序提供了一个可以永久存储数据，并且是使用沙盒机制，其它程序无法访问的独立存储。

### 操作独立存储的文件目录： 

	// 获取独立存储
	IsolatedStorageFile iso = IsolatedStorageFile.GetUserStoreForApplication();
	iso.FileExists			// 判断文件是否存在
	iso.CreateFile  		// 创建文件
	iso.DeleteFile			// 删除文件
	iso.OpenFile			// 打开文件
	iso.MoveFile			// 移动文件
	iso.DirectoryExists		// 判断目录是否存在
	iso.CreateDirectory		// 创建目录
	iso.DeleteDirectory		// 删除目录
	iso.MoveDirectory		// 移动目录
		
### 获取独立存储的文件流：  

打开或者创建文件都会返回一个独立存储的文件流类IsolatedStorageFileStream，如：  

	IsolatedStorageFileStream isfs = iso.CreateFile("File Path");
	IsolatedStorageFileStream isfs = iso.OpenFile("File Path", FileMode.OpenOrCreate);

### 操作独立存储的文件流：  

	// 写入字符串
	using(StreamWriter streamWriter = new StreamWriter(isfs))
	{
		streamWriter.WriteLine("some text");
		streamWriter.Close();
	}

	// 二进制的格式写入
	using(BinaryWriter binaryWriter = new BinaryWriter(isfs))
	{
		binaryWriter.Write("some text");
		binaryWriter.Close();
	}

	// 读取字符串
	string str = string.Empty;
	using (StreamReader streamReader = new StreamReader(isfs))
	{
		str = streamReader.ReadToEnd();
		streamReader.Close();
	}

	// 二进制的格式读取
	using(BinaryReader binaryReader = new BinaryReader(isfs))
	{
		str = binaryReader.ReadString();
		binaryReader.Close();
	}

## 安装文件夹  

工程引入的资源文件存放的位置就是安装文件夹，主要有两类：一类属性为Resource，一类属性为Content，两者的区别是属性为Resource的文件最终会被打包进dll文件中，安装文件夹中只能找到dll文件，属性为Content的文件则不会被打包进dll文件中，并可以在安装文件夹中找到。

### 读取属性为Resource的资源文件：  

	string str = string.Empty;
	Uri uri = new Uri("/Test;component/Resources/test.txt", UriKind.Relative);
	// 获取资源文件流
	StreamResourceInfo streamResourceInfo = Application.GetResourceStream(uri);
	// 构造文件读取流，读取文件字符串
	using (StreamReader streamReader = new StreamReader(streamResourceInfo.Stream))
	{
	    if (streamReader != null)
	    {
	        str = streamReader.ReadToEnd();
	        streamReader.Close();
	    }
	}

如果构造文件写入流，试图写入文本到文件里面，程序会抛出`System.ArgumentException`的异常，异常信息为：`Stream was not writable`，这是因为工程文件不支持写入操作。

### 使用属性为Content的资源文件：  

属性为Content的资源文件一般通过Uri的方式引用到，例如：  

	// 读取属性为Content的图片文件
	BitmapImage m_bitmapImage = new BitmapImage();
	m_bitmapImage.UriSource = new Uri("/Resources/test.png", UriKind.Relative);
	// 读取属性为Content的数据库文件
	TestDataContext db = new TestDataContext("Data Source = 'appdata:/Resources/test.sdf'; File Mode = read only;");

### 访问安装文件夹： 

Windows Phone 8支持访问安装文件夹，提供了访问程序安装文件夹的API，通过Windows.ApplicationModel和Windows.Storage两个命名空间的API可实现对安装文件夹的访问。

	// 获取安装文件夹对象：
	StorageFolder installedLocation = Package.Current.InstalledLocation;

	// 获取当前文件夹下的所有文件夹
	IReadOnlyList<StorageFolder> folderList = await installedLocation.GetFoldersAsync();

	// 获取当前文件夹下的所有文件
	IReadOnlyList<StorageFile> fileList = await installedLocation.GetFilesAsync();

	// 获取当前文件夹下指定名称的文件夹
	StorageFolder sFolder = await installedLocation.GetFolderAsync("Folder Name");

	// 获取当前文件夹下指定名称的文件
	StorageFile sFile = await installedLocation.GetFileAsync("File Name");

	// 在当前文件夹下创建指定名称的文件夹
	StorageFolder sFolder = await installedLocation.CreateFolderAsync("Folder Name");

	// 在当前文件夹下创建指定名称的文件
	StorageFile sFile = await installedLocation.CreateFileAsync("File Name");

	// 删除当前文件夹
	installedLocation.DeleteAsync();

## SD卡  

Windows Phone 8支持程序访问SD卡，或者程序映射到SD卡中特定的文件类型，打开特定文件时系统会启动程序处理该文件。另外，**程序对SD卡文件的访问权限只有读的权限，没有写的权限**。

### 程序访问SD卡：  

Windows Phone 8支持程序通过Microsoft.Phone.Storage命名空间的API访问SD卡。
Microsoft.Phone.Storage命名空间有四个类：  

	ExternalStorage			//提供对 SD 卡的只读访问权限。
	ExternalStorageDevice	//表示插入在手机中的 SD 卡。
	ExternalStorageFile		//表示一个 SD 卡上的文件。
	ExternalStorageFolder	//表示一个 SD 卡上的文件夹。
  
这四个类的使用示例如下：

	// 获取手机的SD卡：
	ExternalStorageDevice esCard = (await ExternalStorage.GetExternalStorageDevicesAsync()).FirstOrDefault();

	// 获取SD卡指定路径的文件：	
	ExternalStorageFile esFile = await esCard.GetFileAsync("File Path");

	// 获取SD卡指定路径的文件夹：
	ExternalStorageFolder esFolder = await _sdCard.GetFolderAsync("Folder Path");

	// 获取文件夹下的所有文件： 
	IEnumerable<ExternalStorageFile> esFiles = await routesFolder.GetFilesAsync();

	// 打开文件流：
	Stream stream = await esFile.OpenForReadAsync();

### SD卡的文件启动程序：  

Windows Phone 8支持程序映射到SD卡中特定的文件类型，打开特定文件时系统会启动程序处理该文件。程序处理SD卡中的特定文件主要有以下几个步骤：

获取访问SD卡的权限  

在WMAppManifest.xml文件中加入：  

	<Capability Name="ID_CAP_REMOVABLE_STORAGE" />

注册扩展名  

打开WMAppManifest.xml文件，在Tokens元素之后的Extensions元素内，使用FileTypeAssociation元素指定文件关联扩展，这里映射的是PDF文件。  

	FileTypeAssociation TaskID="_default" Name="PDF" NavUriFragment="fileToken=%s">
	  <Logos>
	    <Logo Size="small" IsRelative="true">Assets/pdf_Logo33x33.png</Logo>
	    <Logo Size="medium" IsRelative="true">Assets/pdf_Logo69x69.png</Logo>
	    <Logo Size="large" IsRelative="true">Assets/pdf_Logo176x176.png</Logo>
	  </Logos>
	  <SupportedFileTypes>
	    <FileType ContentType="application/pdf">.pdf</FileType>
	  </SupportedFileTypes>
	</FileTypeAssociation>

映射启动URI  

新建一个继承UriMapperBase的子类，并实现MapUri方法。  

	class MyURIMapper : UriMapperBase
	{
	    private string tempUri;

	    public override Uri MapUri(Uri uri)
	    {
	        tempUri = uri.ToString();
	        if (tempUri.Contains("/FileTypeAssociation"))
	        {
	            int fileIDIndex = tempUri.IndexOf("fileToken=") + 10;
	            string fileID = tempUri.Substring(fileIDIndex);

	            return new Uri("/MainPage.xaml?fileToken=" + fileID, UriKind.Relative);
	        }
	        return uri;
	    }
	}

在App类的InitializePhoneApplication方法中，给RootFrame的UriMapper属性赋值。

	private void InitializePhoneApplication()
	{
	    if (phoneApplicationInitialized)
	        return;

	    // Create the frame but don't set it as RootVisual yet; this allows the splash
	    // screen to remain active until the application is ready to render.
	    RootFrame = new PhoneApplicationFrame();
	    RootFrame.Navigated += CompleteInitializePhoneApplication;

	    // Assign the URI-mapper class to the application frame.
	    RootFrame.UriMapper = new MyURIMapper();


	    // Handle navigation failures
	    RootFrame.NavigationFailed += RootFrame_NavigationFailed;

	    // Handle reset requests for clearing the backstack
	    RootFrame.Navigated += CheckForResetNavigation;

	    // Ensure we don't initialize again
	    phoneApplicationInitialized = true;
	}

处理URI参数  

在MainPage类的OnNavigatedTo方法中，处理传进来的URI参数。  

	protected override async void OnNavigatedTo(System.Windows.Navigation.NavigationEventArgs e)
	{
	    if (NavigationContext.QueryString.ContainsKey("fileToken"))
	    {
	        string fileToken = NavigationContext.QueryString["fileToken"];
	        //通过文件标记处理文件
	        await ProcessPDFFile(fileToken);
	    }
	}

处理文件  

	public async Task ProcessPDFFile(string fileToken)
	{
		// 在独立存储中创建文件夹
	    IStorageFolder sFolder = await ApplicationData.Current.LocalFolder.CreateFolderAsync("PDF_Files", CreationCollisionOption.OpenIfExists);

	    // 通过传递进来的文件标记获取文件的名称
	    string fileName = SharedStorageAccessManager.GetSharedFileName(fileToken);

	    // 复制该文件到刚刚创建的文件夹中
	    IStorageFile sFile = await SharedStorageAccessManager.CopySharedFileAsync((StorageFolder)sFolder, fileName, NameCollisionOption.GenerateUniqueName, fileToken);

	    // 打开文件流
	    Stream stream = await sFile.OpenReadAsync();

	    // 处理文件流
	    HandlePDFStream(stream);
	}

## 参考  

1.	<http://msdn.microsoft.com/library/windows/apps/BR227230>
2.	<http://www.cnblogs.com/lipan/archive/2013/04/27/3047130.html>
3.	<http://msdn.microsoft.com/zh-CN/library/windowsphone/develop/ff402541(v=vs.105).aspx>
4.	<http://msdn.microsoft.com/zh-cn/library/windowsphone/develop/jj720573(v=vs.105).aspx>