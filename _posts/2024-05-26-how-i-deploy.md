---
title: How I Deploy My Web Apps (Blazor + ASP.NET)
date: 2024-05-26 12:00:00 +0800
category: Showcase
tags: showcase dotnet 
image:
  path: /images/how-i-deploy-my-web-apps.png
  width: 1280   # in pixels
  height: 720   # in pixels
---

# How I Deploy My Web Apps (Blazor + ASP.NET)

> In summary, I use a one-click utility app (which I created myself) to compile, transmit, and extract deliverables.

Deploying web applications can often be a tedious task, but with the right tools and processes, it can become a seamless and automated experience. Here’s how I deploy my web apps using [Blazor](https://dotnet.microsoft.com/en-us/apps/aspnet/web-apps/blazor) and [ASP.NET](https://dotnet.microsoft.com/en-us/apps/aspnet), avoiding traditional methods like FTP and remote access tools.

## Initial Deployment

For the initial deployment of my web app, I follow a straightforward process to set up the necessary infrastructure:
1. **Set Up the Hosting Environment**: Configure the web server, ensure the .NET runtime is installed, and set up the necessary directories and permissions.
2. **Deploy the Application**: Manually upload the application files to the server, configure [IIS](https://www.iis.net), and perform any required setup tasks.

This initial step ensures that the environment is correctly configured and ready for automated deployments.

## Automated Deployment Utility
I've developed an automated deployment utility app using ASP.NET with a Blazor front end that greatly simplifies my deployment process. This utility performs the entire deployment in a continuous process without requiring any human intervention. Here's what it can do:
- **Compile Selected Projects**: The utility compiles the selected projects, ensuring that both the client-side (Blazor) and server-side (ASP.NET) components are built correctly.
- **File Comparison**: It compares the newly compiled files with those currently on the destination server to identify any differences.
- **Compress Differences**: The utility compresses only the files that have changed, creating a zip file to minimize data transfer.
- **Send Compressed File**: It sends the compressed file to the destination server.
- **Extract Compressed File**: The destination server automatically extracts the contents of the compressed file, updating the necessary files.
- **Restart Server**: Finally, the destination server restarts to apply the changes.

This streamlined approach ensures that the deployment process is efficient and error-free, freeing up my time and allowing my team to focus on development.

## Detailed Deployment Steps
The deployment is structured as follows:
1. **Push to [GitHub](https://github.com)**: Teammates push their source code to the designated GitHub repository.
2. **Run the Utility**: This is the Automated Deployment Utility app mentioned in the previous section.
3. **Select Deployment Options**: Within the utility, I select:
    - The project to compile.
    - Which parts of the project to compile (client-side Blazor and/or server-side ASP.NET).
    - The destination server (alpha, beta, or production).
4. **Compile and Deploy**: After making selections, I click "Compile." The utility remembers the previous choices for later sessions and performs the following steps:
    - **Compile Projects**: Compiles the chosen projects and their dependencies.
    - **File Comparison**: Compares the compiled files with those on the server.
    - **Create Zip File**: Compresses the differences into a zip file to avoid transmitting unchanged files.
    - **Send Zip File**: Sends the zip file to the target server.
    - **Extract and Overwrite**: The target server extracts the zip file, overwriting the existing files.
    - **Restart Application**: The server application restarts to apply the changes.
    - **Notification and Logging**: The local utility posts a notification when the process is complete and logs each step in real-time.

## Benefits of This Approach
- **Efficiency**: The automated deployment process saves time and reduces the risk of errors associated with manual deployments.
- **Consistency**: Ensures that each deployment follows the same steps and configurations, leading to a more stable production environment.
- **Collaboration**: Allows my teammates to focus on coding, knowing that the deployment process is reliable and automated.
- **Security**: Data transmission is done through the same HTTPS channel as the one used by the web app to serve its content to end users. This eliminates the need for remote access tools that might open security issues and avoids the need to open FTP ports, which might be restricted in some corporate environments.
- **Shared Hosting Friendly**: My deployment method is fully compatible with shared hosting because it does not need additional access other than the ones provided by almost all Windows shared hosting web hosts.

The entire process takes less than 2 minutes (usually 30 seconds) and, once started, requires no further intervention. This method saves a lot of time for me, my teammates, and the end users. Since it is a continuous process, I don't need to wait for sub-processes to complete, freeing up my time for other tasks.

By leveraging Blazor and ASP.NET, I’ve created a robust deployment pipeline that simplifies the process and enhances productivity. This approach ensures that my web applications are always up-to-date with the latest changes and improvements from the development team.
