---
title: Multi-Factor Authentication in ASP.net Core
author: damienbod
description: Erfahren Sie, wie Sie die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) in einer ASP.net Core-App einrichten.
monikerRange: '>= aspnetcore-3.1'
ms.author: rick-anderson
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Identity
uid: security/authentication/mfa
ms.openlocfilehash: 6220688d53f0718ca5be5f63dd5d9539d37e2391
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/18/2020
ms.locfileid: "79520146"
---
# <a name="multi-factor-authentication-in-aspnet-core"></a><span data-ttu-id="1518b-103">Multi-Factor Authentication in ASP.net Core</span><span class="sxs-lookup"><span data-stu-id="1518b-103">Multi-factor authentication in ASP.NET Core</span></span>

<span data-ttu-id="1518b-104">Von [Damien Bowder](https://github.com/damienbod)</span><span class="sxs-lookup"><span data-stu-id="1518b-104">By [Damien Bowden](https://github.com/damienbod)</span></span>

<span data-ttu-id="1518b-105">Die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) ist ein Prozess, bei dem ein Benutzer während eines Anmelde Ereignisses für zusätzliche Identifikationsformen angefordert wird.</span><span class="sxs-lookup"><span data-stu-id="1518b-105">Multi-factor authentication (MFA) is a process in which a user is requested during a sign-in event for additional forms of identification.</span></span> <span data-ttu-id="1518b-106">Diese Aufforderung kann darin bestehen, einen Code von einem Mobiltelefon einzugeben, eine FIDO2-Taste zu verwenden oder eine Fingerabdruck Überprüfung bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="1518b-106">This prompt could be to enter a code from a cellphone, use a FIDO2 key, or to provide a fingerprint scan.</span></span> <span data-ttu-id="1518b-107">Wenn Sie eine zweite Form der Authentifizierung benötigen, wird die Sicherheit verbessert.</span><span class="sxs-lookup"><span data-stu-id="1518b-107">When you require a second form of authentication, security is enhanced.</span></span> <span data-ttu-id="1518b-108">Der zusätzliche Faktor ist nicht leicht von einem Angreifer abgerufen oder dupliziert.</span><span class="sxs-lookup"><span data-stu-id="1518b-108">The additional factor isn't easily obtained or duplicated by an attacker.</span></span>

<span data-ttu-id="1518b-109">In diesem Artikel werden die folgenden Bereiche behandelt:</span><span class="sxs-lookup"><span data-stu-id="1518b-109">This article covers the following areas:</span></span>

* <span data-ttu-id="1518b-110">Was ist MFA und welche MFA-Flows werden empfohlen?</span><span class="sxs-lookup"><span data-stu-id="1518b-110">What is MFA and what MFA flows are recommended</span></span>
* <span data-ttu-id="1518b-111">Konfigurieren von MFA für Verwaltungs Seiten mithilfe von ASP.net Core Identity</span><span class="sxs-lookup"><span data-stu-id="1518b-111">Configure MFA for administration pages using ASP.NET Core Identity</span></span>
* <span data-ttu-id="1518b-112">MFA-Anmelde Anforderung an OpenID Connect-Server senden</span><span class="sxs-lookup"><span data-stu-id="1518b-112">Send MFA sign-in requirement to OpenID Connect server</span></span>
* <span data-ttu-id="1518b-113">Erzwingen, dass ASP.net Core OpenID Connect-Client MFA erfordert</span><span class="sxs-lookup"><span data-stu-id="1518b-113">Force ASP.NET Core OpenID Connect client to require MFA</span></span>

## <a name="mfa-2fa"></a><span data-ttu-id="1518b-114">MFA, 2FA</span><span class="sxs-lookup"><span data-stu-id="1518b-114">MFA, 2FA</span></span>

<span data-ttu-id="1518b-115">MFA erfordert mindestens zwei Arten von Beweisen für eine Identität, z. b. etwas, das Sie wissen, etwas, das Sie besitzen, oder die biometrische Validierung für den Benutzer.</span><span class="sxs-lookup"><span data-stu-id="1518b-115">MFA requires at least two or more types of proof for an identity like something you know, something you possess, or biometric validation for the user to authenticate.</span></span>

<span data-ttu-id="1518b-116">Die zweistufige Authentifizierung (2FA) ist wie eine Teilmenge der MFA, aber der Unterschied, dass MFA zwei oder mehr Faktoren erfordern kann, um die Identität zu belegen.</span><span class="sxs-lookup"><span data-stu-id="1518b-116">Two-factor authentication (2FA) is like a subset of MFA, but the difference being that MFA can require two or more factors to prove the identity.</span></span>

### <a name="mfa-totp-time-based-one-time-password-algorithm"></a><span data-ttu-id="1518b-117">MFA TOTP (zeitbasierter Algorithmus für einmal Kennwort)</span><span class="sxs-lookup"><span data-stu-id="1518b-117">MFA TOTP (Time-based One-time Password Algorithm)</span></span>

<span data-ttu-id="1518b-118">MFA mit TOTP ist eine unterstützte Implementierung, die ASP.net Core Identityverwendet.</span><span class="sxs-lookup"><span data-stu-id="1518b-118">MFA using TOTP is a supported implementation using ASP.NET Core Identity.</span></span> <span data-ttu-id="1518b-119">Dies kann in Verbindung mit jeder kompatiblen Authentifikator-App verwendet werden, einschließlich:</span><span class="sxs-lookup"><span data-stu-id="1518b-119">This can be used together with any compliant authenticator app, including:</span></span>

* <span data-ttu-id="1518b-120">Microsoft Authenticator-App</span><span class="sxs-lookup"><span data-stu-id="1518b-120">Microsoft Authenticator App</span></span>
* <span data-ttu-id="1518b-121">Google Authenticator-App</span><span class="sxs-lookup"><span data-stu-id="1518b-121">Google Authenticator App</span></span>

<span data-ttu-id="1518b-122">Weitere Implementierungsdetails finden Sie unter folgendem Link:</span><span class="sxs-lookup"><span data-stu-id="1518b-122">See the following link for implementation details:</span></span>

[<span data-ttu-id="1518b-123">Aktivieren der QR-Code Generierung für TOTP Authenticator-apps in ASP.net Core</span><span class="sxs-lookup"><span data-stu-id="1518b-123">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>](xref:security/authentication/identity-enable-qrcodes)

### <a name="mfa-fido2-or-passwordless"></a><span data-ttu-id="1518b-124">MFA FIDO2 oder passworeless</span><span class="sxs-lookup"><span data-stu-id="1518b-124">MFA FIDO2 or passwordless</span></span>

<span data-ttu-id="1518b-125">FIDO2 ist zurzeit:</span><span class="sxs-lookup"><span data-stu-id="1518b-125">FIDO2 is currently:</span></span>

* <span data-ttu-id="1518b-126">Die sicherste Methode zum Erreichen von MFA.</span><span class="sxs-lookup"><span data-stu-id="1518b-126">The most secure way of achieving MFA.</span></span>
* <span data-ttu-id="1518b-127">Der einzige MFA-Flow, der vor Phishingangriffen schützt.</span><span class="sxs-lookup"><span data-stu-id="1518b-127">The only MFA flow that protects against phishing attacks.</span></span>

<span data-ttu-id="1518b-128">Derzeit unterstützt ASP.net Core FIDO2 nicht direkt.</span><span class="sxs-lookup"><span data-stu-id="1518b-128">At present, ASP.NET Core doesn't support FIDO2 directly.</span></span> <span data-ttu-id="1518b-129">FIDO2 kann für MFA-oder passworeless-Flows verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1518b-129">FIDO2 can be used for MFA or passwordless flows.</span></span>

<span data-ttu-id="1518b-130">Azure Active Directory bietet Unterstützung für FIDO2-und passworeless-Flows.</span><span class="sxs-lookup"><span data-stu-id="1518b-130">Azure Active Directory provides support for FIDO2 and passwordless flows.</span></span> <span data-ttu-id="1518b-131">Weitere Informationen finden Sie unter [passworeless Authentication Options for Azure Active Directory](/azure/active-directory/authentication/concept-authentication-passwordless).</span><span class="sxs-lookup"><span data-stu-id="1518b-131">For more information, see [Passwordless authentication options for Azure Active Directory](/azure/active-directory/authentication/concept-authentication-passwordless).</span></span>

### <a name="mfa-sms"></a><span data-ttu-id="1518b-132">MFA-SMS</span><span class="sxs-lookup"><span data-stu-id="1518b-132">MFA SMS</span></span>

<span data-ttu-id="1518b-133">MFA mit SMS erhöht die Sicherheit im Vergleich zur Kenn Wort Authentifizierung (einzelner Faktor).</span><span class="sxs-lookup"><span data-stu-id="1518b-133">MFA with SMS increases security massively compared with password authentication (single factor).</span></span> <span data-ttu-id="1518b-134">Die Verwendung von SMS als zweiten Faktor ist jedoch nicht mehr empfehlenswert.</span><span class="sxs-lookup"><span data-stu-id="1518b-134">However, using SMS as a second factor is no longer recommended.</span></span> <span data-ttu-id="1518b-135">Für diese Art der Implementierung sind zu viele bekannte Angriffsvektoren vorhanden.</span><span class="sxs-lookup"><span data-stu-id="1518b-135">Too many known attack vectors exist for this type of implementation.</span></span>

[<span data-ttu-id="1518b-136">NIST-Richtlinien</span><span class="sxs-lookup"><span data-stu-id="1518b-136">NIST guidelines</span></span>](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="configure-mfa-for-administration-pages-using-aspnet-core-opno-locidentity"></a><span data-ttu-id="1518b-137">Konfigurieren von MFA für Verwaltungs Seiten mithilfe von ASP.net Core Identity</span><span class="sxs-lookup"><span data-stu-id="1518b-137">Configure MFA for administration pages using ASP.NET Core Identity</span></span>

<span data-ttu-id="1518b-138">MFA kann für Benutzer erzwungen werden, um auf sensible Seiten innerhalb einer ASP.net Core Identity-App zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="1518b-138">MFA could be forced on users to access sensitive pages within an ASP.NET Core Identity app.</span></span> <span data-ttu-id="1518b-139">Dies kann bei apps nützlich sein, bei denen unterschiedliche Zugriffsebenen für die unterschiedlichen Identitäten vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="1518b-139">This could be useful for apps where different levels of access exist for the different identities.</span></span> <span data-ttu-id="1518b-140">Beispielsweise können Benutzer die Profildaten möglicherweise mithilfe einer Kenn Wort Anmeldung anzeigen, aber ein Administrator muss MFA verwenden, um auf die Verwaltungs Seiten zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="1518b-140">For example, users might be able to view the profile data using a password login, but an administrator would be required to use MFA to access the administrative pages.</span></span>

### <a name="extend-the-login-with-an-mfa-claim"></a><span data-ttu-id="1518b-141">Erweitern der Anmeldung mit einem MFA-Anspruch</span><span class="sxs-lookup"><span data-stu-id="1518b-141">Extend the login with an MFA claim</span></span>

<span data-ttu-id="1518b-142">Der Democode ist das Setup mithilfe von ASP.net Core mit Identity und Razor pages.</span><span class="sxs-lookup"><span data-stu-id="1518b-142">The demo code is setup using ASP.NET Core with Identity and Razor Pages.</span></span> <span data-ttu-id="1518b-143">Die `AddIdentity`-Methode wird anstelle `AddDefaultIdentity` eines verwendet, sodass eine `IUserClaimsPrincipalFactory`-Implementierung verwendet werden kann, um nach einer erfolgreichen Anmeldung Ansprüche zur Identität hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="1518b-143">The `AddIdentity` method is used instead of `AddDefaultIdentity` one, so an `IUserClaimsPrincipalFactory` implementation can be used to add claims to the identity after a successful login.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(
            Configuration.GetConnectionString("DefaultConnection")));
    
    services.AddIdentity<IdentityUser, IdentityRole>(
            options => options.SignIn.RequireConfirmedAccount = false)
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddSingleton<IEmailSender, EmailSender>();
    services.AddScoped<IUserClaimsPrincipalFactory<IdentityUser>, 
        AdditionalUserClaimsPrincipalFactory>();

    services.AddAuthorization(options =>
        options.AddPolicy("TwoFactorEnabled",
            x => x.RequireClaim("amr", "mfa")));

    services.AddRazorPages();
}
```

<span data-ttu-id="1518b-144">Die `AdditionalUserClaimsPrincipalFactory`-Klasse fügt dem Benutzer Ansprüche den `amr` Anspruch nur nach einer erfolgreichen Anmeldung hinzu.</span><span class="sxs-lookup"><span data-stu-id="1518b-144">The `AdditionalUserClaimsPrincipalFactory` class adds the `amr` claim to the user claims only after a successful login.</span></span> <span data-ttu-id="1518b-145">Der Wert des Anspruchs wird aus der Datenbank gelesen.</span><span class="sxs-lookup"><span data-stu-id="1518b-145">The claim's value is read from the database.</span></span> <span data-ttu-id="1518b-146">Der Anspruch wird hier hinzugefügt, da der Benutzer nur auf die höhere geschützte Ansicht zugreifen sollte, wenn die Identität mit MFA angemeldet ist.</span><span class="sxs-lookup"><span data-stu-id="1518b-146">The claim is added here because the user should only access the higher protected view if the identity has logged in with MFA.</span></span> <span data-ttu-id="1518b-147">Wenn die Daten Bank Sicht direkt aus der Datenbank gelesen wird, anstatt den Anspruch zu verwenden, ist es möglich, ohne MFA direkt nach Aktivierung der MFA auf die Ansicht zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="1518b-147">If the database view is read from the database directly instead of using the claim, it's possible to access the view without MFA directly after activating the MFA.</span></span>

```csharp
using Microsoft.AspNetCore.Identity;
using Microsoft.Extensions.Options;
using System.Collections.Generic;
using System.Security.Claims;
using System.Threading.Tasks;

namespace IdentityStandaloneMfa
{
    public class AdditionalUserClaimsPrincipalFactory : 
        UserClaimsPrincipalFactory<IdentityUser, IdentityRole>
    {
        public AdditionalUserClaimsPrincipalFactory( 
            UserManager<IdentityUser> userManager,
            RoleManager<IdentityRole> roleManager, 
            IOptions<IdentityOptions> optionsAccessor) 
            : base(userManager, roleManager, optionsAccessor)
        {
        }

        public async override Task<ClaimsPrincipal> CreateAsync(IdentityUser user)
        {
            var principal = await base.CreateAsync(user);
            var identity = (ClaimsIdentity)principal.Identity;

            var claims = new List<Claim>();

            if (user.TwoFactorEnabled)
            {
                claims.Add(new Claim("amr", "mfa"));
            }
            else
            {
                claims.Add(new Claim("amr", "pwd"));
            }

            identity.AddClaims(claims);
            return principal;
        }
    }
}
```

<span data-ttu-id="1518b-148">Da sich die Identity-Dienst Einrichtung in der `Startup`-Klasse geändert hat, müssen die Layouts der Identity aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="1518b-148">Because the Identity service setup changed in the `Startup` class, the layouts of the Identity need to be updated.</span></span> <span data-ttu-id="1518b-149">Gerüstbau der Identity Seiten in der app.</span><span class="sxs-lookup"><span data-stu-id="1518b-149">Scaffold the Identity pages into the app.</span></span> <span data-ttu-id="1518b-150">Definieren Sie das Layout in der Datei *Identity/Account/Manage/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="1518b-150">Define the layout in the *Identity/Account/Manage/_Layout.cshtml* file.</span></span>

```cshtml
@{
    Layout = "/Pages/Shared/_Layout.cshtml";
}
```

<span data-ttu-id="1518b-151">Weisen Sie das Layout auch für alle Seiten verwalten auf den Identity Seiten zu:</span><span class="sxs-lookup"><span data-stu-id="1518b-151">Also assign the layout for all the manage pages from the Identity pages:</span></span>

```cshtml
@{
    Layout = "_Layout.cshtml";
}
```

### <a name="validate-the-mfa-requirement-in-the-administration-page"></a><span data-ttu-id="1518b-152">Überprüfen der MFA-Anforderung auf der Verwaltungsseite</span><span class="sxs-lookup"><span data-stu-id="1518b-152">Validate the MFA requirement in the administration page</span></span>

<span data-ttu-id="1518b-153">Die Razor page "Administration" überprüft, ob der Benutzer sich mithilfe von MFA angemeldet hat.</span><span class="sxs-lookup"><span data-stu-id="1518b-153">The administration Razor Page validates that the user has logged in using MFA.</span></span> <span data-ttu-id="1518b-154">In der `OnGet`-Methode wird die Identität verwendet, um auf die Benutzer Ansprüche zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="1518b-154">In the `OnGet` method, the identity is used to access the user claims.</span></span> <span data-ttu-id="1518b-155">Der `amr`-Anspruch wird auf den Wert `mfa`geprüft.</span><span class="sxs-lookup"><span data-stu-id="1518b-155">The `amr` claim is checked for the value `mfa`.</span></span> <span data-ttu-id="1518b-156">Wenn die Identität diesen Anspruch fehlt oder `false`, wird die Seite an die Seite MFA aktivieren umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="1518b-156">If the identity is missing this claim or is `false`, the page redirects to the Enable MFA page.</span></span> <span data-ttu-id="1518b-157">Dies ist möglich, da sich der Benutzer bereits angemeldet hat, aber ohne MFA.</span><span class="sxs-lookup"><span data-stu-id="1518b-157">This is possible because the user has logged in already, but without MFA.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

namespace IdentityStandaloneMfa
{
    public class AdminModel : PageModel
    {
        public IActionResult OnGet()
        {
            var claimTwoFactorEnabled = 
                User.Claims.FirstOrDefault(t => t.Type == "amr");

            if (claimTwoFactorEnabled != null && 
                "mfa".Equals(claimTwoFactorEnabled.Value))
            {
                // You logged in with MFA, do the administrative stuff
            }
            else
            {
                return Redirect(
                    "/Identity/Account/Manage/TwoFactorAuthentication");
            }

            return Page();
        }
    }
}
```

### <a name="ui-logic-to-toggle-user-login-information"></a><span data-ttu-id="1518b-158">UI-Logik zum Umschalten von Benutzer Anmelde Informationen</span><span class="sxs-lookup"><span data-stu-id="1518b-158">UI logic to toggle user login information</span></span>

<span data-ttu-id="1518b-159">Beim Start wurde eine Autorisierungs Richtlinie hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="1518b-159">An authorization policy was added at startup.</span></span> <span data-ttu-id="1518b-160">Die Richtlinie erfordert den `amr` Anspruch mit dem Wert `mfa`.</span><span class="sxs-lookup"><span data-stu-id="1518b-160">The policy requires the `amr` claim with the value `mfa`.</span></span>

```csharp
services.AddAuthorization(options =>
    options.AddPolicy("TwoFactorEnabled",
        x => x.RequireClaim("amr", "mfa")));
```

<span data-ttu-id="1518b-161">Diese Richtlinie kann dann in der Ansicht `_Layout` verwendet werden, um das Menü **Admin** mit der folgenden Warnung anzuzeigen oder auszublenden:</span><span class="sxs-lookup"><span data-stu-id="1518b-161">This policy can then be used in the `_Layout` view to show or hide the **Admin** menu with the warning:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Identity
@inject SignInManager<IdentityUser> SignInManager
@inject UserManager<IdentityUser> UserManager
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="1518b-162">Wenn die Identität mit MFA angemeldet wurde, wird das Menü **Admin** ohne die QuickInfo-Warnung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1518b-162">If the identity has logged in using MFA, the **Admin** menu is displayed without the tooltip warning.</span></span> <span data-ttu-id="1518b-163">Wenn sich der Benutzer ohne MFA angemeldet hat, wird das Menü **Admin (nicht aktiviert)** zusammen mit der QuickInfo angezeigt, die den Benutzer darüber informiert (die Warnung wird erläutert).</span><span class="sxs-lookup"><span data-stu-id="1518b-163">When the user has logged in without MFA, the **Admin (Not Enabled)** menu is displayed along with the tooltip that informs the user (explaining the warning).</span></span>

```cshtml
@if (SignInManager.IsSignedIn(User))
{
    @if ((AuthorizationService.AuthorizeAsync(User, "TwoFactorEnabled")).Result.Succeeded)
    {
        <li class="nav-item">
            <a class="nav-link text-dark" asp-area="" asp-page="/Admin">Admin</a>
        </li>
    }
    else
    {
        <li class="nav-item">
            <a class="nav-link text-dark" asp-area="" asp-page="/Admin" 
               id="tooltip-demo"  
               data-toggle="tooltip" 
               data-placement="bottom" 
               title="MFA is NOT enabled. This is required for the Admin Page. If you have activated MFA, then logout, login again.">
                Admin (Not Enabled)
            </a>
        </li>
    }
}
```

<span data-ttu-id="1518b-164">Wenn sich der Benutzer ohne MFA anmeldet, wird die Warnung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="1518b-164">If the user logs in without MFA, the warning is displayed:</span></span>

![MFA-Administrator Authentifizierung](mfa/_static/identitystandalonemfa_01.png)

<span data-ttu-id="1518b-166">Wenn Sie auf den Link **Admin** klicken, wird der Benutzer zur MFA-Aktivierungs Ansicht umgeleitet:</span><span class="sxs-lookup"><span data-stu-id="1518b-166">The user is redirected to the MFA enable view when clicking the **Admin** link:</span></span>

![Administrator aktiviert die MFA-Authentifizierung](mfa/_static/identitystandalonemfa_02.png)

## <a name="send-mfa-sign-in-requirement-to-openid-connect-server"></a><span data-ttu-id="1518b-168">MFA-Anmelde Anforderung an OpenID Connect-Server senden</span><span class="sxs-lookup"><span data-stu-id="1518b-168">Send MFA sign-in requirement to OpenID Connect server</span></span> 

<span data-ttu-id="1518b-169">Der `acr_values`-Parameter kann verwendet werden, um den `mfa` erforderlichen Wert vom Client an den Server in einer Authentifizierungsanforderung zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="1518b-169">The `acr_values` parameter can be used to pass the `mfa` required value from the client to the server in an authentication request.</span></span>

> [!NOTE]
> <span data-ttu-id="1518b-170">Der `acr_values`-Parameter muss auf dem Open ID Connect-Server behandelt werden, damit dies funktioniert.</span><span class="sxs-lookup"><span data-stu-id="1518b-170">The `acr_values` parameter needs to be handled on the Open ID Connect server for this to work.</span></span>

### <a name="openid-connect-aspnet-core-client"></a><span data-ttu-id="1518b-171">OpenID Connect-ASP.net Core Client</span><span class="sxs-lookup"><span data-stu-id="1518b-171">OpenID Connect ASP.NET Core client</span></span>

<span data-ttu-id="1518b-172">Der ASP.net Core Razor Pages Open ID Connect-Client-App verwendet die `AddOpenIdConnect`-Methode, um sich beim Open ID Connect-Server anzumelden.</span><span class="sxs-lookup"><span data-stu-id="1518b-172">The ASP.NET Core Razor Pages Open ID Connect client app uses the `AddOpenIdConnect` method to login to the Open ID Connect server.</span></span> <span data-ttu-id="1518b-173">Der `acr_values`-Parameter wird mit dem `mfa` Wert festgelegt und mit der Authentifizierungsanforderung gesendet.</span><span class="sxs-lookup"><span data-stu-id="1518b-173">The `acr_values` parameter is set with the `mfa` value and sent with the authentication request.</span></span> <span data-ttu-id="1518b-174">Der `OpenIdConnectEvents` wird zum Hinzufügen dieses verwendet.</span><span class="sxs-lookup"><span data-stu-id="1518b-174">The `OpenIdConnectEvents` is used to add this.</span></span>

<span data-ttu-id="1518b-175">Informationen zu den empfohlenen `acr_values`-Parameterwerten finden Sie unter [Referenzwerte für die Authentifizierungsmethode](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08).</span><span class="sxs-lookup"><span data-stu-id="1518b-175">For recommended `acr_values` parameter values, see [Authentication Method Reference Values](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08).</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(options =>
    {
        options.DefaultScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme =
            OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.SignInScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.Authority = "<OpenID Connect server URL>";
        options.RequireHttpsMetadata = true;
        options.ClientId = "<OpenID Connect client ID>";
        options.ClientSecret = "<>";
        // Code with PKCE can also be used here
        options.ResponseType = "code id_token";
        options.Scope.Add("profile");
        options.Scope.Add("offline_access");
        options.SaveTokens = true;
        options.Events = new OpenIdConnectEvents
        {
            OnRedirectToIdentityProvider = context =>
            {
                context.ProtocolMessage.SetParameter("acr_values", "mfa");
                return Task.FromResult(0);
            }
        };
    });
```

### <a name="example-openid-connect-identityserver-4-server-with-aspnet-core-opno-locidentity"></a><span data-ttu-id="1518b-176">Beispiel für einen OpenID Connect identityserver 4-Server mit ASP.net Core Identity</span><span class="sxs-lookup"><span data-stu-id="1518b-176">Example OpenID Connect IdentityServer 4 server with ASP.NET Core Identity</span></span>

<span data-ttu-id="1518b-177">Auf dem OpenID Connect-Server, der mit ASP.net Core Identity mit MVC-Ansichten implementiert wird, wird eine neue Ansicht mit dem Namen *ErrorEnable2FA. cshtml* erstellt.</span><span class="sxs-lookup"><span data-stu-id="1518b-177">On the OpenID Connect server, which is implemented using ASP.NET Core Identity with MVC views, a new view named *ErrorEnable2FA.cshtml* is created.</span></span> <span data-ttu-id="1518b-178">Die Ansicht:</span><span class="sxs-lookup"><span data-stu-id="1518b-178">The view:</span></span>

* <span data-ttu-id="1518b-179">Zeigt an, ob das Identity aus einer APP stammt, die MFA erfordert, der Benutzer dies jedoch nicht in Identityaktiviert hat.</span><span class="sxs-lookup"><span data-stu-id="1518b-179">Displays if the Identity comes from an app that requires MFA but the user hasn't activated this in Identity.</span></span>
* <span data-ttu-id="1518b-180">Informiert den Benutzer und fügt einen Link hinzu, um diesen zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="1518b-180">Informs the user and adds a link to activate this.</span></span>

```cshtml
@{
    ViewData["Title"] = "ErrorEnable2FA";
}

<h1>The client application requires you to have MFA enabled. Enable this, try login again.</h1>

<br />

You can enable MFA to login here:

<br />

<a asp-controller="Manage" asp-action="TwoFactorAuthentication">Enable MFA</a>
```

<span data-ttu-id="1518b-181">In der `Login`-Methode wird die `IIdentityServerInteractionService` Schnittstellen Implementierung `_interaction` für den Zugriff auf die Open ID Connect-Anforderungs Parameter verwendet.</span><span class="sxs-lookup"><span data-stu-id="1518b-181">In the `Login` method, the `IIdentityServerInteractionService` interface implementation `_interaction` is used to access the Open ID Connect request parameters.</span></span> <span data-ttu-id="1518b-182">Auf den `acr_values`-Parameter wird mit der `AcrValues`-Eigenschaft zugegriffen.</span><span class="sxs-lookup"><span data-stu-id="1518b-182">The `acr_values` parameter is accessed using the `AcrValues` property.</span></span> <span data-ttu-id="1518b-183">Da der Client dies mit `mfa` Satz gesendet hat, kann dies aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="1518b-183">As the client sent this with `mfa` set, this can then be checked.</span></span>

<span data-ttu-id="1518b-184">Wenn MFA erforderlich ist und für den Benutzer in ASP.net Core Identity MFA aktiviert ist, wird die Anmeldung fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="1518b-184">If MFA is required, and the user in ASP.NET Core Identity has MFA enabled, then the login continues.</span></span> <span data-ttu-id="1518b-185">Wenn für den Benutzer keine MFA aktiviert ist, wird der Benutzer zur benutzerdefinierten Ansicht *ErrorEnable2FA. cshtml*umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="1518b-185">When the user has no MFA enabled, the user is redirected to the custom view *ErrorEnable2FA.cshtml*.</span></span> <span data-ttu-id="1518b-186">Anschließend ASP.net Core Identity den Benutzer signiert.</span><span class="sxs-lookup"><span data-stu-id="1518b-186">Then ASP.NET Core Identity signs the user in.</span></span>

```csharp
//
// POST: /Account/Login
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Login(LoginInputModel model)
{
    var returnUrl = model.ReturnUrl;
    var context = 
        await _interaction.GetAuthorizationContextAsync(returnUrl);
    var requires2Fa = 
        context?.AcrValues.Count(t => t.Contains("mfa")) >= 1;

    var user = await _userManager.FindByNameAsync(model.Email);
    if (user != null && !user.TwoFactorEnabled && requires2Fa)
    {
        return RedirectToAction(nameof(ErrorEnable2FA));
    }

    // code omitted for brevity
```

<span data-ttu-id="1518b-187">Die `ExternalLoginCallback`-Methode funktioniert wie die lokale Identity Anmeldung.</span><span class="sxs-lookup"><span data-stu-id="1518b-187">The `ExternalLoginCallback` method works like the local Identity login.</span></span> <span data-ttu-id="1518b-188">Die `AcrValues`-Eigenschaft wird auf den `mfa` Wert geprüft.</span><span class="sxs-lookup"><span data-stu-id="1518b-188">The `AcrValues` property is checked for the `mfa` value.</span></span> <span data-ttu-id="1518b-189">Wenn der `mfa` Wert vorhanden ist, wird MFA erzwungen, bevor die Anmeldung abgeschlossen ist (z. b. an die `ErrorEnable2FA` Ansicht umgeleitet).</span><span class="sxs-lookup"><span data-stu-id="1518b-189">If the `mfa` value is present, MFA is forced before the login completes (for example, redirected to the `ErrorEnable2FA` view).</span></span>

```csharp
//
// GET: /Account/ExternalLoginCallback
[HttpGet]
[AllowAnonymous]
public async Task<IActionResult> ExternalLoginCallback(
    string returnUrl = null,
    string remoteError = null)
{
    var context =
        await _interaction.GetAuthorizationContextAsync(returnUrl);
    var requires2Fa =
        context?.AcrValues.Count(t => t.Contains("mfa")) >= 1;

    if (remoteError != null)
    {
        ModelState.AddModelError(
            string.Empty,
            _sharedLocalizer["EXTERNAL_PROVIDER_ERROR", 
            remoteError]);
        return View(nameof(Login));
    }
    var info = await _signInManager.GetExternalLoginInfoAsync();

    if (info == null)
    {
        return RedirectToAction(nameof(Login));
    }

    var email = info.Principal.FindFirstValue(ClaimTypes.Email);

    if (!string.IsNullOrEmpty(email))
    {
        var user = await _userManager.FindByNameAsync(email);
        if (user != null && !user.TwoFactorEnabled && requires2Fa)
        {
            return RedirectToAction(nameof(ErrorEnable2FA));
        }
    }

    // Sign in the user with this external login provider if the user already has a login.
    var result = await _signInManager
        .ExternalLoginSignInAsync(
            info.LoginProvider, 
            info.ProviderKey, 
            isPersistent: 
            false);

    // code omitted for brevity
```

<span data-ttu-id="1518b-190">Wenn der Benutzer bereits angemeldet ist, wird die Client-App:</span><span class="sxs-lookup"><span data-stu-id="1518b-190">If the user is already logged in, the client app:</span></span>

* <span data-ttu-id="1518b-191">Überprüft weiterhin den `amr` Anspruch.</span><span class="sxs-lookup"><span data-stu-id="1518b-191">Still validates the `amr` claim.</span></span>
* <span data-ttu-id="1518b-192">Die MFA kann mit einem Link zum ASP.net Core Identity Ansicht eingerichtet werden.</span><span class="sxs-lookup"><span data-stu-id="1518b-192">Can set up the MFA with a link to the ASP.NET Core Identity view.</span></span>

![acr_values-1](mfa/_static/acr_values-1.png)

## <a name="force-aspnet-core-openid-connect-client-to-require-mfa"></a><span data-ttu-id="1518b-194">Erzwingen, dass ASP.net Core OpenID Connect-Client MFA erfordert</span><span class="sxs-lookup"><span data-stu-id="1518b-194">Force ASP.NET Core OpenID Connect client to require MFA</span></span>

<span data-ttu-id="1518b-195">Dieses Beispiel zeigt, wie eine ASP.net Core Razor Page-APP, die OpenID Connect zum Anmelden verwendet, erfordern kann, dass sich Benutzer mithilfe von MFA authentifiziert haben.</span><span class="sxs-lookup"><span data-stu-id="1518b-195">This example shows how an ASP.NET Core Razor Page app, which uses OpenID Connect to sign in, can require that users have authenticated using MFA.</span></span>

<span data-ttu-id="1518b-196">Um die MFA-Anforderung zu überprüfen, wird eine `IAuthorizationRequirement` Anforderung erstellt.</span><span class="sxs-lookup"><span data-stu-id="1518b-196">To validate the MFA requirement, an `IAuthorizationRequirement` requirement is created.</span></span> <span data-ttu-id="1518b-197">Diese wird den Seiten mithilfe einer Richtlinie hinzugefügt, die MFA erfordert.</span><span class="sxs-lookup"><span data-stu-id="1518b-197">This will be added to the pages using a policy that requires MFA.</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
 
namespace AspNetCoreRequireMfaOidc
{
    public class RequireMfa : IAuthorizationRequirement{}
}
```

<span data-ttu-id="1518b-198">Eine `AuthorizationHandler` wird implementiert, die den `amr`-Anspruch verwendet und auf den Wert `mfa`prüft.</span><span class="sxs-lookup"><span data-stu-id="1518b-198">An `AuthorizationHandler` is implemented that will use the `amr` claim and check for the value `mfa`.</span></span> <span data-ttu-id="1518b-199">Der `amr` wird in der `id_token` einer erfolgreichen Authentifizierung zurückgegeben und kann viele verschiedene Werte aufweisen, wie in der Spezifikation der [Referenzwerte der Authentifizierungsmethode](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08) definiert.</span><span class="sxs-lookup"><span data-stu-id="1518b-199">The `amr` is returned in the `id_token` of a successful authentication and can have many different values as defined in the [Authentication Method Reference Values](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08) specification.</span></span>

<span data-ttu-id="1518b-200">Der zurückgegebene Wert hängt von der Authentifizierung der Identität und der Open ID Connect-Server Implementierung ab.</span><span class="sxs-lookup"><span data-stu-id="1518b-200">The returned value depends on how the identity authenticated and on the Open ID Connect server implementation.</span></span>

<span data-ttu-id="1518b-201">Der `AuthorizationHandler` verwendet die `RequireMfa` Anforderung und überprüft den `amr` Anspruch.</span><span class="sxs-lookup"><span data-stu-id="1518b-201">The `AuthorizationHandler` uses the `RequireMfa` requirement and validates the `amr` claim.</span></span> <span data-ttu-id="1518b-202">Der OpenID Connect-Server kann mit IdentityServer4 mit ASP.net Core Identityimplementiert werden.</span><span class="sxs-lookup"><span data-stu-id="1518b-202">The OpenID Connect server can be implemented using IdentityServer4 with ASP.NET Core Identity.</span></span> <span data-ttu-id="1518b-203">Wenn ein Benutzer sich mit TOTP anmeldet, wird der `amr` Anspruch mit einem MFA-Wert zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="1518b-203">When a user logs in using TOTP, the `amr` claim is returned with an MFA value.</span></span> <span data-ttu-id="1518b-204">Wenn eine andere OpenID Connect-Server Implementierung oder ein anderer MFA-Typ verwendet wird, kann der `amr` Anspruch einen anderen Wert aufweisen.</span><span class="sxs-lookup"><span data-stu-id="1518b-204">If using a different OpenID Connect server implementation or a different MFA type, the `amr` claim will, or can, have a different value.</span></span> <span data-ttu-id="1518b-205">Der Code muss erweitert werden, um dies ebenfalls zu akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="1518b-205">The code must be extended to accept this as well.</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace AspNetCoreRequireMfaOidc
{
    public class RequireMfaHandler : AuthorizationHandler<RequireMfa>
    {
        protected override Task HandleRequirementAsync(
            AuthorizationHandlerContext context, 
            RequireMfa requirement)
        {
            if (context == null)
                throw new ArgumentNullException(nameof(context));
            if (requirement == null)
                throw new ArgumentNullException(nameof(requirement));

            var amrClaim =
                context.User.Claims.FirstOrDefault(t => t.Type == "amr");

            if (amrClaim != null && amrClaim.Value == Amr.Mfa)
            {
                context.Succeed(requirement);
            }

            return Task.CompletedTask;
        }
    }
}
```

<span data-ttu-id="1518b-206">In der `Startup.ConfigureServices`-Methode wird die `AddOpenIdConnect`-Methode als Standard Aufforderungs Schema verwendet.</span><span class="sxs-lookup"><span data-stu-id="1518b-206">In the `Startup.ConfigureServices` method, the `AddOpenIdConnect` method is used as the default challenge scheme.</span></span> <span data-ttu-id="1518b-207">Der Autorisierungs Handler, der zum Überprüfen des `amr` Anspruchs verwendet wird, wird der Inversion des Steuerelement Containers hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="1518b-207">The authorization handler, which is used to check the `amr` claim, is added to the Inversion of Control container.</span></span> <span data-ttu-id="1518b-208">Anschließend wird eine Richtlinie erstellt, mit der die `RequireMfa` Anforderung hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="1518b-208">A policy is then created which adds the `RequireMfa` requirement.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.ConfigureApplicationCookie(options =>
        options.Cookie.SecurePolicy =
            CookieSecurePolicy.Always);

    services.AddSingleton<IAuthorizationHandler, RequireMfaHandler>();

    services.AddAuthentication(options =>
    {
        options.DefaultScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme =
            OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.SignInScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.Authority = "https://localhost:44352";
        options.RequireHttpsMetadata = true;
        options.ClientId = "AspNetCoreRequireMfaOidc";
        options.ClientSecret = "AspNetCoreRequireMfaOidcSecret";
        options.ResponseType = "code id_token";
        options.Scope.Add("profile");
        options.Scope.Add("offline_access");
        options.SaveTokens = true;
    });

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireMfa", policyIsAdminRequirement =>
        {
            policyIsAdminRequirement.Requirements.Add(new RequireMfa());
        });
    });

    services.AddRazorPages();
}
```

<span data-ttu-id="1518b-209">Diese Richtlinie wird dann bei Bedarf auf der Razor-Seite verwendet.</span><span class="sxs-lookup"><span data-stu-id="1518b-209">This policy is then used in the Razor page as required.</span></span> <span data-ttu-id="1518b-210">Die Richtlinie könnte auch global für die gesamte app hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="1518b-210">The policy could be added globally for the entire app as well.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;
using Microsoft.Extensions.Logging;

namespace AspNetCoreRequireMfaOidc.Pages
{
    [Authorize(Policy= "RequireMfa")]
    public class IndexModel : PageModel
    {
        private readonly ILogger<IndexModel> _logger;

        public IndexModel(ILogger<IndexModel> logger)
        {
            _logger = logger;
        }

        public void OnGet()
        {
        }
    }
}
```

<span data-ttu-id="1518b-211">Wenn der Benutzer ohne MFA authentifiziert wird, hat der `amr` Anspruch wahrscheinlich einen `pwd` Wert.</span><span class="sxs-lookup"><span data-stu-id="1518b-211">If the user authenticates without MFA, the `amr` claim will probably have a `pwd` value.</span></span> <span data-ttu-id="1518b-212">Die Anforderung wird nicht autorisiert, auf die Seite zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="1518b-212">The request won't be authorized to access the page.</span></span> <span data-ttu-id="1518b-213">Wenn Sie die Standardwerte verwenden, wird der Benutzer auf die Seite *Account/AccessDenied* umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="1518b-213">Using the default values, the user will be redirected to the *Account/AccessDenied* page.</span></span> <span data-ttu-id="1518b-214">Dieses Verhalten kann geändert werden, oder Sie können hier eine eigene benutzerdefinierte Logik implementieren.</span><span class="sxs-lookup"><span data-stu-id="1518b-214">This behavior can be changed or you can implement your own custom logic here.</span></span> <span data-ttu-id="1518b-215">In diesem Beispiel wird ein Link hinzugefügt, sodass der gültige Benutzer MFA für sein Konto einrichten kann.</span><span class="sxs-lookup"><span data-stu-id="1518b-215">In this example, a link is added so that the valid user can set up MFA for their account.</span></span>

```cshtml
@page
@model AspNetCoreRequireMfaOidc.AccessDeniedModel
@{
    ViewData["Title"] = "AccessDenied";
    Layout = "~/Pages/Shared/_Layout.cshtml";
}

<h1>AccessDenied</h1>

You require MFA to login here

<a href="https://localhost:44352/Manage/TwoFactorAuthentication">Enable MFA</a>
```

<span data-ttu-id="1518b-216">Jetzt können nur Benutzer, die sich mit MFA authentifizieren, auf die Seite oder Website zugreifen.</span><span class="sxs-lookup"><span data-stu-id="1518b-216">Now only users that authenticate with MFA can access the page or website.</span></span> <span data-ttu-id="1518b-217">Wenn verschiedene MFA-Typen verwendet werden oder wenn 2FA okay ist, weist der `amr` Anspruch unterschiedliche Werte auf und muss ordnungsgemäß verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="1518b-217">If different MFA types are used or if 2FA is okay, the `amr` claim will have different values and needs to be processed correctly.</span></span> <span data-ttu-id="1518b-218">Verschiedene Open ID Connect-Server geben auch andere Werte für diesen Anspruch zurück und folgen möglicherweise nicht der Spezifikation der [Referenzwerte der Authentifizierungsmethode](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08) .</span><span class="sxs-lookup"><span data-stu-id="1518b-218">Different Open ID Connect servers also return different values for this claim and might not follow the [Authentication Method Reference Values](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08) specification.</span></span>

<span data-ttu-id="1518b-219">Bei der Anmeldung ohne MFA (z. b. nur mit einem Kennwort):</span><span class="sxs-lookup"><span data-stu-id="1518b-219">When logging in without MFA (for example, using just a password):</span></span>

* <span data-ttu-id="1518b-220">Der `amr` hat den `pwd` Wert:</span><span class="sxs-lookup"><span data-stu-id="1518b-220">The `amr` has the `pwd` value:</span></span>

    ![require_mfa_oidc_02. png](mfa/_static/require_mfa_oidc_02.png)

* <span data-ttu-id="1518b-222">Zugriff verweigert:</span><span class="sxs-lookup"><span data-stu-id="1518b-222">Access is denied:</span></span>

    ![require_mfa_oidc_03. png](mfa/_static/require_mfa_oidc_03.png)

<span data-ttu-id="1518b-224">Alternativ dazu können Sie sich mit OTP bei Identityanmelden:</span><span class="sxs-lookup"><span data-stu-id="1518b-224">Alternatively, logging in using OTP with Identity:</span></span>

![require_mfa_oidc_01. png](mfa/_static/require_mfa_oidc_01.png)

## <a name="additional-resources"></a><span data-ttu-id="1518b-226">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="1518b-226">Additional resources</span></span>

* [<span data-ttu-id="1518b-227">Aktivieren der QR-Code Generierung für TOTP Authenticator-apps in ASP.net Core</span><span class="sxs-lookup"><span data-stu-id="1518b-227">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>](xref:security/authentication/identity-enable-qrcodes)
* [<span data-ttu-id="1518b-228">Nicht verarbeitlose Authentifizierungs Optionen für Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1518b-228">Passwordless authentication options for Azure Active Directory</span></span>](/azure/active-directory/authentication/concept-authentication-passwordless)
* [<span data-ttu-id="1518b-229">FIDO2 .NET-Bibliothek für FIDO2/webauthn-Nachweis und-unter Verwendung von .net</span><span class="sxs-lookup"><span data-stu-id="1518b-229">FIDO2 .NET library for FIDO2 / WebAuthn Attestation and Assertion using .NET</span></span>](https://github.com/abergs/fido2-net-lib)
* [<span data-ttu-id="1518b-230">Webauthn großartig</span><span class="sxs-lookup"><span data-stu-id="1518b-230">WebAuthn Awesome</span></span>](https://github.com/herrjemand/awesome-webauthn)