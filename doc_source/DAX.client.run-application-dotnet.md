# \.NET and DAX<a name="DAX.client.run-application-dotnet"></a>

**Note**  
The following tutorial uses the \.NET Core SDK, and shows how you can run a program in your default Amazon VPC to access your DAX cluster\.  
If you prefer, you can use the AWS Toolkit for Visual Studio to write a \.NET application and deploy it into your VPC\. For more information, see [Creating and Deploying Elastic Beanstalk Applications in \.NET Using AWS Toolkit for Visual Studio](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_NET.html) in the *AWS Elastic Beanstalk Developer Guide*\.

To run the \.NET sample on your Amazon EC2 instance, follow this proceedure:

1. Go to [https://www\.microsoft\.com/net/download/linux](https://www.microsoft.com/net/download/linux) and download the latest \.NET Core SDK for Linux\. The downloaded file will be named as follows: `dotnet-sdk-N.N.N-linux-x64.tar.gz`

1. Extract the \.NET Core files:

   ```
   mkdir dotnet
   tar zxvf dotnet-sdk-N.N.N-linux-x64.tar.gz -C dotnet
   ```

   Replace `N.N.N` with the actual version number of the \.NET Core SDK\. For example: `2.1.4`

1. Verify the installation:

   ```
   alias dotnet=$HOME/dotnet/dotnet
   dotnet --version
   ```

   This should print the version number of the \.NET Core SDK\.
**Note**  
Instead of the versioun number, you might receive the following error:  
`error: libunwind.so.8: cannot open shared object file: No such file or directory`  
To resolve the error, install the `libunwind` package:  

   ```
   sudo yum install -y libunwind
   ```
After you do this, you should be able to run the `dotnet --version` command without any errors\.

1. Create a new \.NET project:

   ```
   dotnet new console -o myApp 
   ```

   This will require a few minutes to perform a one\-time\-only setup\. When it completes, run the sample project:

   ```
   dotnet run --project myApp
   ```

   You should receive the following message: `Hello World!`

1. The `myApp/myApp.csproj` file contains metadata about your project\. To use the DAX client in your application, you will need to modify the file so that it looks like this:

   ```
   <Project Sdk="Microsoft.NET.Sdk">
   
     <PropertyGroup>
       <OutputType>Exe</OutputType>
       <TargetFramework>netcoreapp2.0</TargetFramework>
     </PropertyGroup>
     <ItemGroup>
       <PackageReference Include="AWSSDK.DAX.Client" Version="1.0.0" />
     </ItemGroup>
   
   </Project>
   ```

1. Download the sample program source code \(`.zip` file\):

   ```
   wget http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/samples/TryDax.zip
   ```

   When the download is complete, extract the source files:

   ```
   unzip TryDax.zip
   ```

1. You will now run the sample programs, one at a time\. For each program, you will copy its contents into the `myApp/Program.cs`, and then run the `MyApp` project\.\.

   Run the following \.NET programs:

   ```
   cp 01-CreateTable.cs myApp/Program.cs
   dotnet run --project myApp
   
   cp 02-Write-Data.cs myApp/Program.cs
   dotnet run --project myApp
   ```

   The first program creates a DynamoDB table named `TryDaxTable`\. The second program writes data to the table\.

1. You will now some programs to perform `GetItem`, `Query` and `Scan` operations on your DAX cluster\. To determine the endpoint for your DAX cluster, choose one of the following:
   +  **Using the DynamoDB console**—choose your DAX cluster\. The cluster endpoint is shown in the console\. For example:

     ```
     mycluster.frfx8h.clustercfg.dax.amazonaws.com:8111
     ```
   + **Using the AWS CLI**—type the following command:

     ```
     aws dax describe-clusters --query "Clusters[*].ClusterDiscoveryEndpoint"
     ```

      The cluster endpoint port and address are shown in the output\. For example: 

     ```
     {
         "Port": 8111, 
         "Address":"mycluster.frfx8h.clustercfg.dax.amazonaws.com"
     }
     ```

   Now run the following programs, specifying your cluster endpoint as a command line parameter\. \(Replace the sample endpoint with your actual DAX cluster endpoint\.\)

   ```
   cp 03-GetItem-Test.cs myApp/Program.cs
   dotnet run --project myApp mycluster.frfx8h.clustercfg.dax.amazonaws.com:8111
   
   cp 04-Query-Test.cs myApp/Program.cs
   dotnet run --project myApp mycluster.frfx8h.clustercfg.dax.amazonaws.com:8111
   
   cp 05-Scan-Test.cs myApp/Program.cs
   dotnet run --project myApp mycluster.frfx8h.clustercfg.dax.amazonaws.com:8111
   ```

   Take note of the timing information—the number of milliseconds required for the `GetItem`, `Query` and `Scan` tests\.

1. Run the following \.NET program to delete `TryDaxTable`:

   ```
   cp 06-DeleteTable.cs myApp/Program.cs
   dotnet run --project myApp
   ```

For more information about these programs, see the following sections:
+ [01\-CreateTable\.cs](DAX.client.run-application-dotnet.01-CreateTable.md)
+ [02\-Write\-Data\.cs](DAX.client.run-application-dotnet.02-Write-Data.md)
+ [03\-GetItem\-Test\.cs](DAX.client.run-application-dotnet.03-GetItem-Test.md)
+ [04\-Query\-Test\.cs](DAX.client.run-application-dotnet.04-Query-Test.md)
+ [05\-Scan\-Test\.cs](DAX.client.run-application-dotnet.05-Scan-Test.md)
+ [06\-DeleteTable\.cs](DAX.client.run-application-dotnet.06-DeleteTable.md)