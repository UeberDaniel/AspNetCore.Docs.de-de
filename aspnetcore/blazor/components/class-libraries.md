---
title: Klassenbibliotheken für ASP.NET Core-Razor-Komponenten
author: guardrex
description: Erfahren Sie, wie Komponenten aus einer externen Komponentenbibliothek in Blazor-Apps einbezogen werden können.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/23/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/components/class-libraries
ms.openlocfilehash: b54eb7142fc7e1665cc0aaaad068a67852ac2f74
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85399074"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="5ee3d-103">Klassenbibliotheken für ASP.NET Core-Razor-Komponenten</span><span class="sxs-lookup"><span data-stu-id="5ee3d-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="5ee3d-104">Von [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="5ee3d-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="5ee3d-105">Komponenten können in einer [Razor-Klassenbibliothek (RCL)](xref:razor-pages/ui-class) projektübergreifend gemeinsam genutzt werden.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="5ee3d-106">Eine *Klassenbibliothek für Razor-Komponenten* kann wie folgt einbezogen werden:</span><span class="sxs-lookup"><span data-stu-id="5ee3d-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="5ee3d-107">Über ein anderes Projekt in der Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-107">Another project in the solution.</span></span>
* <span data-ttu-id="5ee3d-108">Über ein NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-108">A NuGet package.</span></span>
* <span data-ttu-id="5ee3d-109">Über eine referenzierte .NET-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-109">A referenced .NET library.</span></span>

<span data-ttu-id="5ee3d-110">Genauso wie Komponenten normale .NET-Typen sind, sind von der RCL bereitgestellte Komponenten normale .NET-Assemblys.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="5ee3d-111">Erstellen einer RCL</span><span class="sxs-lookup"><span data-stu-id="5ee3d-111">Create an RCL</span></span>

<span data-ttu-id="5ee3d-112">Befolgen Sie die Anweisungen im Artikel <xref:blazor/get-started>, um Ihre Umgebung für Blazor zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="5ee3d-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ee3d-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="5ee3d-114">Erstellen Sie ein neues Projekt.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-114">Create a new project.</span></span>
1. <span data-ttu-id="5ee3d-115">Klicken Sie auf **Razor-Klassenbibliothek**.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-115">Select **Razor Class Library**.</span></span> <span data-ttu-id="5ee3d-116">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-116">Select **Next**.</span></span>
1. <span data-ttu-id="5ee3d-117">Klicken Sie im Dialogfeld **Neue Razor-Klassenbibliothek erstellen** auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-117">In the **Create a new Razor class library** dialog, select **Create**.</span></span>
1. <span data-ttu-id="5ee3d-118">Geben Sie im Feld **Projektname** einen Projektnamen ein, oder übernehmen Sie den Standardnamen.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="5ee3d-119">Die Beispiele in diesem Thema verwenden den Projektnamen `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-119">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="5ee3d-120">Wählen Sie **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-120">Select **Create**.</span></span>
1. <span data-ttu-id="5ee3d-121">Fügen Sie die RCL zu einer Projektmappe hinzu:</span><span class="sxs-lookup"><span data-stu-id="5ee3d-121">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="5ee3d-122">Klicken Sie mit der rechten Maustaste auf die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-122">Right-click the solution.</span></span> <span data-ttu-id="5ee3d-123">Wählen Sie **Hinzufügen** > **Vorhandenes Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-123">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="5ee3d-124">Navigieren Sie zur Projektdatei der RCL.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-124">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="5ee3d-125">Wählen Sie die Projektdatei (`.csproj`) der RCL aus.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-125">Select the RCL's project file (`.csproj`).</span></span>
1. <span data-ttu-id="5ee3d-126">Fügen Sie einen Verweis auf die RCL aus der App hinzu:</span><span class="sxs-lookup"><span data-stu-id="5ee3d-126">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="5ee3d-127">Klicken Sie mit der rechten Maustaste auf das App-Projekt.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-127">Right-click the app project.</span></span> <span data-ttu-id="5ee3d-128">Wählen Sie **Hinzufügen** > **Verweis** aus.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-128">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="5ee3d-129">Wählen Sie das RCL-Projekt aus.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-129">Select the RCL project.</span></span> <span data-ttu-id="5ee3d-130">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-130">Select **OK**.</span></span>

> [!NOTE]
> <span data-ttu-id="5ee3d-131">Wenn das Kontrollkästchen **Seiten und Ansichten unterstützen** beim Erstellen der RCL aus der Vorlage aktiviert ist, fügen Sie auch eine `_Imports.razor`-Datei mit folgendem Inhalt zum Stammverzeichnis des generierten Projekts hinzu, um das Erstellen von Razor-Komponenten zu ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="5ee3d-131">If the **Support pages and views** check box is selected when generating the RCL from the template, then also add an `_Imports.razor` file to root of the generated project with the following contents to enable Razor component authoring:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Components.Web
> ```
>
> <span data-ttu-id="5ee3d-132">Fügen Sie die Datei manuell dem Stammverzeichnis des generierten Projekts hinzu.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-132">Manually add the file the root of the generated project.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="5ee3d-133">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="5ee3d-133">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="5ee3d-134">Verwenden Sie die Vorlage **Razor-Klassenbibliothek** (`razorclasslib`) mit dem Befehl [`dotnet new`](/dotnet/core/tools/dotnet-new) in einer Befehlsshell.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-134">Use the **Razor Class Library** template (`razorclasslib`) with the [`dotnet new`](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="5ee3d-135">Im folgenden Beispiel wird eine RCL mit dem Namen `MyComponentLib1` erstellt.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-135">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="5ee3d-136">Der Ordner, der `MyComponentLib1` enthält, wird automatisch erstellt, wenn der Befehl ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="5ee3d-136">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

   > [!NOTE]
   > <span data-ttu-id="5ee3d-137">Wenn der Switch `-s|--support-pages-and-views` beim Erstellen der RCL aus der Vorlage verwendet wird, fügen Sie auch eine `_Imports.razor`-Datei mit folgendem Inhalt zum Stammverzeichnis des generierten Projekts hinzu, um das Erstellen von Razor-Komponenten zu ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="5ee3d-137">If the `-s|--support-pages-and-views` switch is used when generating the RCL from the template, then also add an `_Imports.razor` file to root of the generated project with the following contents to enable Razor component authoring:</span></span>
   >
   > ```razor
   > @using Microsoft.AspNetCore.Components.Web
   > ```
   >
   > <span data-ttu-id="5ee3d-138">Fügen Sie die Datei manuell dem Stammverzeichnis des generierten Projekts hinzu.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-138">Manually add the file the root of the generated project.</span></span>

1. <span data-ttu-id="5ee3d-139">Verwenden Sie den Befehl [`dotnet add reference`](/dotnet/core/tools/dotnet-add-reference) in einer Befehlsshell, um die Bibliothek zu einem bestehenden Projekt hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-139">To add the library to an existing project, use the [`dotnet add reference`](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="5ee3d-140">Im folgenden Beispiel wird die RCL der App hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-140">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="5ee3d-141">Führen Sie den folgenden Befehl aus dem Projektordner der App mit dem Pfad zur Bibliothek aus:</span><span class="sxs-lookup"><span data-stu-id="5ee3d-141">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a><span data-ttu-id="5ee3d-142">Verwenden einer Bibliothekskomponente</span><span class="sxs-lookup"><span data-stu-id="5ee3d-142">Consume a library component</span></span>

<span data-ttu-id="5ee3d-143">Um Komponenten, die in einer Bibliothek definiert sind, in einem anderen Projekt zu verwenden, verwenden Sie einen der folgenden Ansätze:</span><span class="sxs-lookup"><span data-stu-id="5ee3d-143">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="5ee3d-144">Verwenden Sie den vollständigen Typnamen mit dem Namespace.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-144">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="5ee3d-145">Verwenden Sie die Razor-Anweisung [`@using`](xref:mvc/views/razor#using).</span><span class="sxs-lookup"><span data-stu-id="5ee3d-145">Use Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="5ee3d-146">Einzelne Komponenten können über den Namen hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-146">Individual components can be added by name.</span></span>

<span data-ttu-id="5ee3d-147">In den folgenden Beispielen ist `MyComponentLib1` eine Komponentenbibliothek, die eine `SalesReport`-Komponente enthält.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-147">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="5ee3d-148">Die `SalesReport`-Komponente kann über ihren vollständigen Typnamen mit Namespace referenziert werden:</span><span class="sxs-lookup"><span data-stu-id="5ee3d-148">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```razor
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="5ee3d-149">Auf die Komponente kann auch verwiesen werden, wenn die Bibliothek mit einer `@using`-Direktive in den Gültigkeitsbereich eingebunden wird:</span><span class="sxs-lookup"><span data-stu-id="5ee3d-149">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```razor
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="5ee3d-150">Fügen Sie die `@using MyComponentLib1`-Direktive in die `_Import.razor`-Datei der obersten Ebene ein, um die Komponenten der Bibliothek für ein ganzes Projekt zur Verfügung zu stellen.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-150">Include the `@using MyComponentLib1` directive in the top-level `_Import.razor` file to make the library's components available to an entire project.</span></span> <span data-ttu-id="5ee3d-151">Fügen Sie die Direktive auf beliebiger Ebene zu einer `_Import.razor`-Datei hinzu, um den Namespace auf eine einzelne Seite oder eine Reihe von Seiten innerhalb eines Ordners anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-151">Add the directive to an `_Import.razor` file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="5ee3d-152">Erstellen einer Klassenbibliothek für Razor-Komponenten mit statischen Objekten</span><span class="sxs-lookup"><span data-stu-id="5ee3d-152">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="5ee3d-153">Eine RCL kann statische Objekte enthalten.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-153">An RCL can include static assets.</span></span> <span data-ttu-id="5ee3d-154">Die statischen Objekte sind für jede App verfügbar, die die Bibliothek nutzt.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-154">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="5ee3d-155">Weitere Informationen finden Sie unter <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-155">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="5ee3d-156">Erstellen, Verpacken und Liefern an NuGet</span><span class="sxs-lookup"><span data-stu-id="5ee3d-156">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="5ee3d-157">Da es sich bei Komponentenbibliotheken um standardmäßige .NET-Bibliotheken handelt, unterscheidet sich das Verpacken und Liefern an NuGet nicht vom Verpacken und Liefern einer beliebigen Bibliothek an NuGet.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-157">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="5ee3d-158">Das Verpacken wird mit dem Befehl [`dotnet pack`](/dotnet/core/tools/dotnet-pack) in einer Befehlsshell durchgeführt:</span><span class="sxs-lookup"><span data-stu-id="5ee3d-158">Packaging is performed using the [`dotnet pack`](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```dotnetcli
dotnet pack
```

<span data-ttu-id="5ee3d-159">Laden Sie das Paket mit dem Befehl [`dotnet nuget push`](/dotnet/core/tools/dotnet-nuget-push) in einer Befehlsshell in NuGet hoch.</span><span class="sxs-lookup"><span data-stu-id="5ee3d-159">Upload the package to NuGet using the [`dotnet nuget push`](/dotnet/core/tools/dotnet-nuget-push) command in a command shell.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5ee3d-160">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="5ee3d-160">Additional resources</span></span>

* <xref:razor-pages/ui-class>
* [<span data-ttu-id="5ee3d-161">Hinzufügen einer XML-Linkerkonfigurationsdatei zu einer Bibliothek</span><span class="sxs-lookup"><span data-stu-id="5ee3d-161">Add an XML linker configuration file to a library</span></span>](xref:blazor/host-and-deploy/configure-linker#add-an-xml-linker-configuration-file-to-a-library)