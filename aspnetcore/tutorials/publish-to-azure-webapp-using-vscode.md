---
title: Veröffentlichen einer ASP.NET Core-App in Azure mit Visual Studio Code
author: ricardoserradas
description: Erfahren Sie, wie eine ASP.NET Core-App in Azure App Service mit Visual Studio Code veröffentlicht wird.
ms.author: ricardoserradas
ms.custom: mvc
ms.date: 04/16/2019
uid: tutorials/publish-to-azure-webapp-using-vscode
ms.openlocfilehash: 64d82835f6a47a458802692c99658b964c07f807
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889605"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio-code"></a><span data-ttu-id="2104d-103">Veröffentlichen einer ASP.NET Core-App in Azure mit Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2104d-103">Publish an ASP.NET Core app to Azure with Visual Studio Code</span></span>

<span data-ttu-id="2104d-104">Von [Ricardo Serradas](https://twitter.com/ricardoserradas)</span><span class="sxs-lookup"><span data-stu-id="2104d-104">By [Ricardo Serradas](https://twitter.com/ricardoserradas)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="2104d-105">Informationen zur Problembehandlung bei der App Service-Bereitstellung finden Sie unter <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="2104d-105">To troubleshoot an App Service deployment issue, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="intro"></a><span data-ttu-id="2104d-106">Einführung</span><span class="sxs-lookup"><span data-stu-id="2104d-106">Intro</span></span>

<span data-ttu-id="2104d-107">In diesem Tutorial erfahren Sie, wie Sie eine ASP.Net Core MVC-Anwendung erstellen und diese in Visual Studio Code bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="2104d-107">With this tutorial, you'll learn how to create an ASP.Net Core MVC Application and deploy it within Visual Studio Code.</span></span>

## <a name="set-up"></a><span data-ttu-id="2104d-108">Einrichten</span><span class="sxs-lookup"><span data-stu-id="2104d-108">Set up</span></span>

- <span data-ttu-id="2104d-109">Eröffnen Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/dotnet/), wenn Sie noch über kein Konto verfügen.</span><span class="sxs-lookup"><span data-stu-id="2104d-109">Open a [free Azure account](https://azure.microsoft.com/free/dotnet/) if you don't have one.</span></span>
- <span data-ttu-id="2104d-110">Installieren Sie das [.NET Core SDK](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="2104d-110">Install [.NET Core SDK](https://dotnet.microsoft.com/download)</span></span>
- <span data-ttu-id="2104d-111">Installieren Sie [Visual Studio Code](https://code.visualstudio.com/Download).</span><span class="sxs-lookup"><span data-stu-id="2104d-111">Install [Visual Studio Code](https://code.visualstudio.com/Download)</span></span>
  - <span data-ttu-id="2104d-112">Installieren Sie die [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) für Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2104d-112">Install the [C# Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) to Visual Studio Code</span></span>
  - <span data-ttu-id="2104d-113">Installieren Sie die [Azure App Service-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) für Visual Studio Code und konfigurieren Sie diese, bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="2104d-113">Instal the [Azure App Service Extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) to Visual Studio Code and configure it before proceeding</span></span>

## <a name="create-an-aspnet-core-mvc-project"></a><span data-ttu-id="2104d-114">Erstellen eines ASP.NET Core MVC-Projekts</span><span class="sxs-lookup"><span data-stu-id="2104d-114">Create an ASP.Net Core MVC project</span></span>

<span data-ttu-id="2104d-115">Navigieren Sie bei Verwendung eines Terminals zu dem Ordner, in dem Sie das Projekt erstellen wollen, und verwenden Sie den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="2104d-115">Using a terminal, navigate to the folder you want the project to be created on and use the following command:</span></span>

```cmd
> dotnet new mvc
```

<span data-ttu-id="2104d-116">Es wird Ihnen eine Ordnerstruktur wie die folgende angezeigt:</span><span class="sxs-lookup"><span data-stu-id="2104d-116">You'll have a folder structure similar to the following:</span></span>

```cmd
      appsettings.Development.json
      appsettings.json
<DIR> Controllers
<DIR> Models
      netcore-vscode.csproj
<DIR> obj
      Program.cs
<DIR> Properties
      Startup.cs
<DIR> Views
<DIR> wwwroot
```

## <a name="open-it-with-visual-studio-code"></a><span data-ttu-id="2104d-117">Öffnen des Projekts mit Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2104d-117">Open it with Visual Studio Code</span></span>

<span data-ttu-id="2104d-118">Nachdem das Projekt erstellt wurde, können Sie es mit Visual Studio Code öffnen, indem Sie eine der folgenden Optionen verwenden:</span><span class="sxs-lookup"><span data-stu-id="2104d-118">After your project is created, you can open it with Visual Studio Code by using one of the options below:</span></span>

### <a name="through-the-command-line"></a><span data-ttu-id="2104d-119">Über die Befehlszeile</span><span class="sxs-lookup"><span data-stu-id="2104d-119">Through the command line</span></span>

<span data-ttu-id="2104d-120">Verwenden Sie in dem Ordner, in dem das Projekt erstellt wurde, den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="2104d-120">Use the following command within the folder you created the project:</span></span>

```cmd
> code .
```

<span data-ttu-id="2104d-121">Wenn der oben angegebene Befehl nicht funktioniert, überprüfen Sie, ob Ihre Installation ordnungsgemäß konfiguriert ist, indem Sie auf [diesen Link](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform) verweisen.</span><span class="sxs-lookup"><span data-stu-id="2104d-121">If the command below does not work, check if your installation is configured properly by referencing [this link](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform).</span></span>

### <a name="through-visual-studio-code-interface"></a><span data-ttu-id="2104d-122">Über die Visual Studio Code-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="2104d-122">Through Visual Studio Code interface</span></span>

- <span data-ttu-id="2104d-123">Öffnen Sie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2104d-123">Open Visual Studio Code</span></span>
- <span data-ttu-id="2104d-124">Wählen Sie im Menü `File > Open Folder` aus.</span><span class="sxs-lookup"><span data-stu-id="2104d-124">On the menu, select `File > Open Folder`</span></span>
- <span data-ttu-id="2104d-125">Wählen Sie den Stamm des Ordners aus, in dem das MVC-Projekt erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="2104d-125">Select the root of the folder you created the MVC Project</span></span>

<span data-ttu-id="2104d-126">Wenn Sie den Projektordner öffnen, wird eine Meldung angezeigt, dass erforderliche Ressourcen zum Erstellen und Debuggen nicht vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="2104d-126">When you open the project folder, you'll receive a message saying that required assets to build and debug are missing.</span></span> <span data-ttu-id="2104d-127">Akzeptieren Sie die Hilfe, um sie hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="2104d-127">Accept the help to add them.</span></span>

![Visual Studio Code-Schnittstelle mit geladenem Projekt](publish-to-azure-webapp-using-vscode/_static/folder-structure-restore-netcore.jpg)

<span data-ttu-id="2104d-129">Ein `.vscode`-Ordner wird in der Projektstruktur erstellt.</span><span class="sxs-lookup"><span data-stu-id="2104d-129">A `.vscode` folder will be created under the project structure.</span></span> <span data-ttu-id="2104d-130">Er enthält die folgenden Dateien:</span><span class="sxs-lookup"><span data-stu-id="2104d-130">It will contain the following files:</span></span>

```cmd
launch.json
tasks.json
```

<span data-ttu-id="2104d-131">Hierbei handelt es sich um Hilfsprogrammdateien zum Erstellen und Debuggen Ihrer .NET Core-Web-App.</span><span class="sxs-lookup"><span data-stu-id="2104d-131">These are utility files to help you build and debug your .NET Core Web App.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="2104d-132">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="2104d-132">Run the app</span></span>

<span data-ttu-id="2104d-133">Bevor Sie die App in Azure bereitstellen, sollten Sie sicherstellen, dass sie auf Ihrem lokalen Computer ordnungsgemäß ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2104d-133">Before we deploy the app to Azure, make sure it is running properly on your local machine.</span></span>

- <span data-ttu-id="2104d-134">Drücken Sie F5, um das Projekt auszuführen.</span><span class="sxs-lookup"><span data-stu-id="2104d-134">Press F5 to run the project</span></span>

<span data-ttu-id="2104d-135">Ihre Web-App wird in einem neuen Tab Ihres Standardbrowsers ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2104d-135">Your web app will start running on a new tab of your default browser.</span></span> <span data-ttu-id="2104d-136">Möglicherweise wird beim Starten eine Datenschutzwarnung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2104d-136">You may notice a privacy warning as soon as it starts.</span></span> <span data-ttu-id="2104d-137">Dies liegt daran, dass Ihre App zum Starten entweder HTTP oder HTTPs verwendet, und sie standardmäßig zum HTTPS-Endpunkt navigiert.</span><span class="sxs-lookup"><span data-stu-id="2104d-137">This is because your app will start either using HTTP and HTTPS, and it navigates to the HTTPS endpoint by default.</span></span>

![Datenschutzwarnung beim lokalen Debuggen der App](publish-to-azure-webapp-using-vscode/_static/run-webapp-https-warning.jpg)

<span data-ttu-id="2104d-139">Klicken Sie auf `Advanced` und dann auf `Continue to localhost (unsafe)`, um die Debugsitzung weiter auszuführen.</span><span class="sxs-lookup"><span data-stu-id="2104d-139">To keep the debugging session, click `Advanced` and then `Continue to localhost (unsafe)`.</span></span>

## <a name="generate-the-deployment-package-locally"></a><span data-ttu-id="2104d-140">Bereitstellungspaket lokal generieren</span><span class="sxs-lookup"><span data-stu-id="2104d-140">Generate the deployment package locally</span></span>

- <span data-ttu-id="2104d-141">Öffnen Sie das Visual Studio Code-Terminal.</span><span class="sxs-lookup"><span data-stu-id="2104d-141">Open Visual Studio Code terminal</span></span>
- <span data-ttu-id="2104d-142">Verwenden Sie den folgenden Befehl zum Generieren eines `Release`-Pakets in einem Unterordner namens `publish`:</span><span class="sxs-lookup"><span data-stu-id="2104d-142">Use the following command to generate a `Release` package to a sub folder called `publish`:</span></span>
  - `dotnet publish -c Release -o ./publish`
- <span data-ttu-id="2104d-143">Ein neuer `publish`-Ordner wird in der Projektstruktur erstellt.</span><span class="sxs-lookup"><span data-stu-id="2104d-143">A new `publish` folder will be created under the project structure</span></span>

![Struktur des „publish“-Ordners](publish-to-azure-webapp-using-vscode/_static/publish-folder.jpg)

## <a name="publish-to-azure-app-service"></a><span data-ttu-id="2104d-145">Veröffentlichen in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2104d-145">Publish to Azure App Service</span></span>

<span data-ttu-id="2104d-146">Nutzen Sie die Azure App Service-Erweiterung für Visual Studio Code, und führen Sie die unten aufgeführten Schritte aus, um die Website direkt in Azure App Service zu veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="2104d-146">Leveraging the Azure App Service extension for Visual Studio Code, follow the steps below to publish the website directly to the Azure App Service.</span></span>

### <a name="if-youre-creating-a-new-web-app"></a><span data-ttu-id="2104d-147">Wenn Sie eine neue Web-App erstellen</span><span class="sxs-lookup"><span data-stu-id="2104d-147">If you're creating a new Web App</span></span>

- <span data-ttu-id="2104d-148">Klicken Sie mit der rechten Maustaste auf den `publish`-Ordner, und wählen Sie `Deploy to Web App...` aus.</span><span class="sxs-lookup"><span data-stu-id="2104d-148">Right click the `publish` folder and select `Deploy to Web App...`</span></span>
- <span data-ttu-id="2104d-149">Wählen Sie das Abonnement aus, das Sie zum Erstellen der Web-App verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="2104d-149">Select the subscription you want to create the Web App</span></span>
- <span data-ttu-id="2104d-150">Klicken Sie auf `Create New Web App`.</span><span class="sxs-lookup"><span data-stu-id="2104d-150">Select `Create New Web App`</span></span>
- <span data-ttu-id="2104d-151">Geben Sie einen Namen für die Web-App ein.</span><span class="sxs-lookup"><span data-stu-id="2104d-151">Enter a name for the Web App</span></span>

<span data-ttu-id="2104d-152">Mit dieser Erweiterung wird die neue Web-App erstellt, und das Paket für die App wird automatisch bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="2104d-152">The extension will create the new Web App and will automatically start deploying the package to it.</span></span> <span data-ttu-id="2104d-153">Nachdem die Bereitstellung abgeschlossen ist, klicken Sie auf `Browse Website`, um die Bereitstellung zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="2104d-153">Once the deployment is finished, click `Browse Website` to validate the deployment.</span></span>

![Meldung über erfolgreiche Bereitstellung](publish-to-azure-webapp-using-vscode/_static/deployment-succeeded-message.jpg)

<span data-ttu-id="2104d-155">Durch Klicken auf `Browse Website` gelangen Sie über Ihren Standardbrowser zur App:</span><span class="sxs-lookup"><span data-stu-id="2104d-155">Once you click `Browse Website`, you'll navigate to it using your default browser:</span></span>

![Erfolgreich bereitgestellte neue Web-App](publish-to-azure-webapp-using-vscode/_static/new-webapp-deployed.jpg)

### <a name="if-youre-deploying-to-an-existing-web-app"></a><span data-ttu-id="2104d-157">Wenn Sie Inhalte in einer vorhandenen Web-App bereitstellen</span><span class="sxs-lookup"><span data-stu-id="2104d-157">If you're deploying to an existing Web App</span></span>

- <span data-ttu-id="2104d-158">Klicken Sie mit der rechten Maustaste auf den `publish`-Ordner, und wählen Sie `Deploy to Web App...` aus.</span><span class="sxs-lookup"><span data-stu-id="2104d-158">Right click the `publish` folder and select `Deploy to Web App...`</span></span>
- <span data-ttu-id="2104d-159">Wählen Sie das Abonnement aus, in dem sich die vorhandene Web-App befindet.</span><span class="sxs-lookup"><span data-stu-id="2104d-159">Select the subscription the existing Web App resides</span></span>
- <span data-ttu-id="2104d-160">Wählen Sie die Web-App aus der Liste aus.</span><span class="sxs-lookup"><span data-stu-id="2104d-160">Select the Web App from the list</span></span>
- <span data-ttu-id="2104d-161">Sie werden von Visual Studio Code gefragt, ob die vorhandenen Inhalte überschrieben werden sollen.</span><span class="sxs-lookup"><span data-stu-id="2104d-161">Visual Studio Code will ask you if you want to overwrite the existing content.</span></span> <span data-ttu-id="2104d-162">Klicken Sie zum Bestätigen auf `Deploy`.</span><span class="sxs-lookup"><span data-stu-id="2104d-162">Click `Deploy` to confirm</span></span>

<span data-ttu-id="2104d-163">Die Erweiterung stellt die aktualisierten Inhalte in der Web-App bereit.</span><span class="sxs-lookup"><span data-stu-id="2104d-163">The extension will deploy the updated content to the Web App.</span></span> <span data-ttu-id="2104d-164">Sobald dieser Vorgang abgeschlossen ist, klicken Sie auf `Browse Website`, um die Bereitstellung zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="2104d-164">Once it's done, click `Browse Website` to validate the deployment.</span></span>

![Erfolgreich bereitgestellte Inhalte in vorhandener Web-App](publish-to-azure-webapp-using-vscode/_static/existing-webapp-deployed.jpg)

## <a name="next-steps"></a><span data-ttu-id="2104d-166">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="2104d-166">Next steps</span></span>

- [<span data-ttu-id="2104d-167">Erstellen Ihrer ersten Azure DevOps-Pipeline</span><span class="sxs-lookup"><span data-stu-id="2104d-167">Create your first Azure DevOps pipeline</span></span>](/azure/devops/pipelines/create-first-pipeline)

## <a name="additional-resources"></a><span data-ttu-id="2104d-168">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="2104d-168">Additional resources</span></span>

- [<span data-ttu-id="2104d-169">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2104d-169">Azure App Service</span></span>](/azure/app-service/app-service-web-overview)
- [<span data-ttu-id="2104d-170">Azure-Ressourcengruppen</span><span class="sxs-lookup"><span data-stu-id="2104d-170">Azure resource groups</span></span>](/azure/azure-resource-manager/resource-group-overview#resource-groups)