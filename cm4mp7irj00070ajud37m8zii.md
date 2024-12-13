---
title: "Implementation of Keycloak for Modern Applications"
seoTitle: "Keycloak: Securing Modern Applications"
seoDescription: "Discover Keycloak's identity management for secure apps: setup, features, integration, with examples and code"
datePublished: Fri Dec 13 2024 12:02:16 GMT+0000 (Coordinated Universal Time)
cuid: cm4mp7irj00070ajud37m8zii
slug: implementation-of-keycloak-for-modern-applications
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1734090737849/31baf800-8872-4a34-8ac0-8d5f26758451.png
tags: authentication, sso, keycloak

---


<p><strong>In the digital era, robust identity and access management systems are crucial for secure applications. Keycloak stands out as an open-source solution that simplifies user authentication and authorization with enterprise-grade features.</strong></p>
<p>This blog dives deep into the essentials of Keycloak, guiding you through its configuration and usage with practical examples and code snippets. It also covers how tokens are used in Keycloak for secure communication and how frontend integration works.</p>
<hr>
<h2 id="table-of-contents">Table of Contents</h2>
<ol>
<li><a href="#introduction-to-keycloak">Introduction to Keycloak</a>  </li>
<li><a href="#initial-setup-and-configuration">Initial Setup and Configuration</a>  <ul>
<li><a href="#prerequisites">Prerequisites</a>  </li>
<li><a href="#configuring-keycloak-for-your-application">Configuring Keycloak for Your Application</a>  </li>
</ul>
</li>
<li><a href="#setting-up-keycloak-and-key-features">Setting Up Keycloak and Key Features</a>  <ul>
<li><a href="#accessing-keycloak-admin-console">Accessing Keycloak Admin Console</a>  </li>
<li><a href="#adding-a-user">Adding a User</a>  </li>
<li><a href="#creating-a-client">Creating a Client</a>  </li>
</ul>
</li>
<li><a href="#hands-on-example-keycloak-integration">Hands-On Example: Keycloak Integration</a>  <ul>
<li><a href="#frontend-configuration-angular-example">Frontend Configuration (Angular Example)</a>  </li>
<li><a href="#backend-validation-aspnet-example">Backend Validation (ASP.NET Example)</a>  </li>
</ul>
</li>
<li><a href="#tokenization-frontend-and-backend-integration">Tokenization: Frontend and Backend Integration</a>  <ul>
<li><a href="#frontend-tokenization">Frontend Tokenization</a>  </li>
<li><a href="#backend-token-validation">Backend Token Validation</a>  </li>
</ul>
</li>
<li><a href="#conclusion">Conclusion</a></li>
</ol>
<hr>
<h2 id="introduction-to-keycloak">Introduction to Keycloak</h2>
<p>Keycloak is an open-source identity and access management tool that enables:</p>
<ul>
<li><strong>Single Sign-On (SSO)</strong>: Login once, access multiple applications.</li>
<li><strong>Identity Management</strong>: Centralized control over users, roles, and permissions.</li>
<li><strong>Security</strong>: Advanced features like multi-factor authentication (MFA), social login, and token-based validation using OpenID Connect or OAuth2 protocols.</li>
</ul>
<p>With Keycloak, developers can reduce implementation complexity while ensuring high-security standards.</p>
<hr>
<h2 id="initial-setup-and-configuration">Initial Setup and Configuration</h2>
<h3 id="prerequisites">Prerequisites</h3>
<ol>
<li>A running Keycloak instance.</li>
<li>Admin credentials for Keycloak.</li>
<li>Basic understanding of authentication and authorization.</li>
</ol>
<h3 id="configuring-keycloak-for-your-application">Configuring Keycloak for Your Application</h3>
<p>To integrate Keycloak into your application, update the configuration files like <code>appsettings.json</code>, <code>appsettings.Development.json</code>, and <code>config.js</code> with the following details:</p>
<pre><code class="lang-json"><span class="hljs-comment">"SSO"</span>: {
    <span class="hljs-comment">"AuthType"</span>: <span class="hljs-comment">"SSO"</span>,
    <span class="hljs-comment">"BaseUrl"</span>: <span class="hljs-comment">"http://your-keycloak-url:8080"</span>,
    <span class="hljs-comment">"Realm"</span>: <span class="hljs-comment">"your-realm"</span>,
    <span class="hljs-comment">"ClientId"</span>: <span class="hljs-comment">"your-client-id"</span>,
    <span class="hljs-comment">"Issuer"</span>: <span class="hljs-comment">"keycloakIssuer"</span>,
    <span class="hljs-comment">"Audience"</span>: <span class="hljs-comment">"keycloakIssuer"</span>,
    <span class="hljs-comment">"PublicKey"</span>: <span class="hljs-comment">"&lt;Your_Public_Key_Here&gt;"</span>
}
</code></pre>
<ul>
<li><code>AuthType</code>: Specifies the authentication type (e.g., SSO).</li>
<li><code>BaseUrl</code>: URL of the Keycloak server.</li>
<li><code>Realm</code>: The realm your application belongs to.</li>
<li><code>ClientId</code>: Identifier for your application.</li>
<li><code>Issuer &amp; Audience</code>: Validate token authenticity.</li>
</ul>
<h3 id="token-refresh-configuration">Token Refresh Configuration</h3>
<pre><code class="lang-json"><span class="hljs-string">"KC_TOKENREFRESHINTERVAL"</span>: <span class="hljs-number">300000</span>,
<span class="hljs-string">"KC_TOKENREFRESH_BEFOREEXPIRY"</span>: <span class="hljs-number">60</span>
</code></pre>
<ul>
<li><code>KC_TOKENREFRESHINTERVAL</code>: Time interval (in ms) for refreshing the token.</li>
<li><code>KC_TOKENREFRESH_BEFOREEXPIRY</code>: Time (in seconds) before expiry to request a new token.</li>
</ul>
<hr>
<h2 id="setting-up-keycloak-and-key-features">Setting Up Keycloak and Key Features</h2>
<h3 id="accessing-keycloak-admin-console">Accessing Keycloak Admin Console</h3>
<ol>
<li>Navigate to your Keycloak instance: <code>http://your-keycloak-url:8080/admin</code>.</li>
<li>Log in using admin credentials.</li>
</ol>
<h3 id="adding-a-user">Adding a User</h3>
<ol>
<li>Select the appropriate realm.</li>
<li>Go to <code>Users &gt; Add User</code>.</li>
<li>Enter details (Username, Email, First Name, Last Name) and click Save.</li>
<li>Under the <code>Credentials</code> tab, set a temporary password.</li>
</ol>
<h3 id="creating-a-client">Creating a Client</h3>
<ol>
<li>Navigate to <code>Clients</code> and click <code>Create</code>.</li>
<li>Enter a unique Client ID (e.g., <code>my-web-app</code>) and select Client Protocol as <code>openid-connect</code>.</li>
<li>Configure Redirect URIs for login redirection.</li>
<li>Save and adjust settings like Access Type (public or confidential) and authorization.</li>
</ol>
<hr>
<h2 id="hands-on-example-keycloak-integration">Hands-On Example: Keycloak Integration</h2>
<h3 id="frontend-configuration-angular-example-">Frontend Configuration (Angular Example)</h3>
<p>Add Keycloak initialization in <code>main.ts</code>:</p>
<pre><code class="lang-typescript"><span class="hljs-keyword">import</span> Keycloak <span class="hljs-keyword">from</span> <span class="hljs-string">'keycloak-js'</span>;

<span class="hljs-keyword">const</span> keycloak = <span class="hljs-keyword">new</span> Keycloak({
    <span class="hljs-attr">url</span>: <span class="hljs-string">'http://your-keycloak-url:8080'</span>,
    <span class="hljs-attr">realm</span>: <span class="hljs-string">'your-realm'</span>,
    <span class="hljs-attr">clientId</span>: <span class="hljs-string">'your-client-id'</span>
});

keycloak.init({ <span class="hljs-attr">onLoad</span>: <span class="hljs-string">'login-required'</span> }).then(<span class="hljs-function"><span class="hljs-params">authenticated</span> =&gt;</span> {
    <span class="hljs-built_in">console</span>.log(authenticated ? <span class="hljs-string">'Authenticated'</span> : <span class="hljs-string">'Not authenticated'</span>);
}).catch(<span class="hljs-built_in">console</span>.error);
</code></pre>
<h3 id="backend-validation-asp-net-example-">Backend Validation (ASP.NET Example)</h3>
<p>Use middleware to validate tokens:</p>
<pre><code class="lang-csharp"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(<span class="hljs-keyword">options</span> =&gt;
    {
        <span class="hljs-keyword">options</span>.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
        <span class="hljs-keyword">options</span>.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
    })
    .AddJwtBearer(<span class="hljs-keyword">options</span> =&gt;
    {
        <span class="hljs-keyword">options</span>.Authority = <span class="hljs-string">"http://your-keycloak-url:8080/realms/your-realm"</span>;
        <span class="hljs-keyword">options</span>.Audience = <span class="hljs-string">"your-client-id"</span>;
        <span class="hljs-keyword">options</span>.TokenValidationParameters = <span class="hljs-keyword">new</span> TokenValidationParameters
        {
            ValidateIssuerSigningKey = <span class="hljs-keyword">true</span>,
            ValidateIssuer = <span class="hljs-keyword">true</span>,
            ValidateAudience = <span class="hljs-keyword">true</span>
        };
    });
}
</code></pre>
<hr>
<h2 id="tokenization-frontend-and-backend-integration">Tokenization: Frontend and Backend Integration</h2>
<h3 id="frontend-tokenization">Frontend Tokenization</h3>
<p>To ensure secure communication between your frontend application and Keycloak, you need to obtain and pass an access token. Once the user is authenticated with Keycloak, you can retrieve the token and include it in API requests.</p>
<p>Example:</p>
<pre><code class="lang-typescript"><span class="hljs-keyword">const</span> <span class="hljs-keyword">token</span> = keycloak.<span class="hljs-keyword">token</span>;
</code></pre>
<ul>
<li>Store the token securely (e.g., in memory) and use it for API calls.</li>
<li>Example of passing the token in an HTTP request:</li>
</ul>
<pre><code class="lang-typescript"><span class="hljs-keyword">const</span> headers = <span class="hljs-keyword">new</span> HttpHeaders().<span class="hljs-keyword">set</span>(<span class="hljs-string">'Authorization'</span>, <span class="hljs-string">'Bearer '</span> + token);
<span class="hljs-keyword">this</span>.http.<span class="hljs-keyword">get</span>(<span class="hljs-string">'https://api.yourserver.com/data'</span>, { headers }).subscribe(response =&gt; {
    console.log(response);
});
</code></pre>
<h3 id="backend-token-validation">Backend Token Validation</h3>
<p>In the backend, the token passed from the frontend needs to be validated to ensure the request&#39;s authenticity and integrity.</p>
<p>For example, in an ASP.NET backend, you can configure token validation middleware as shown below:</p>
<pre><code class="lang-csharp"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(<span class="hljs-keyword">options</span> =&gt;
    {
        <span class="hljs-keyword">options</span>.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
        <span class="hljs-keyword">options</span>.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
    })
    .AddJwtBearer(<span class="hljs-keyword">options</span> =&gt;
    {
        <span class="hljs-keyword">options</span>.Authority = <span class="hljs-string">"http://your-keycloak-url:8080/realms/your-realm"</span>;
        <span class="hljs-keyword">options</span>.Audience = <span class="hljs-string">"your-client-id"</span>;
        <span class="hljs-keyword">options</span>.TokenValidationParameters = <span class="hljs-keyword">new</span> TokenValidationParameters
        {
            ValidateIssuerSigningKey = <span class="hljs-keyword">true</span>,
            ValidateIssuer = <span class="hljs-keyword">true</span>,
            ValidateAudience = <span class="hljs-keyword">true</span>
        };
    });
}
</code></pre>
<p>The middleware will automatically validate the token&#39;s issuer, audience, and signature, ensuring that only valid requests are processed.</p>
<hr>
<h2 id="conclusion">Conclusion</h2>
<p>Keycloak stands out as a reliable, robust, and feature-rich identity and access management solution for modern applications. Its capabilities, including Single Sign-On (SSO), Role-Based Access Control (RBAC), Multi-Factor Authentication (MFA), and seamless integration with popular protocols like OAuth2.0 and OpenID Connect, make it an essential tool for securing applications across diverse platforms.</p>
<p>By simplifying the complexities of authentication and authorization, Keycloak empowers organizations to achieve high-security standards while enhancing user experience. Whether you are managing enterprise-scale applications, APIs, or microservices, Keycloak’s scalability and customization options ensure it aligns perfectly with your business needs.</p>
<p>Implementing Keycloak effectively can transform how you manage identity and access, strengthening your security posture and fostering trust among users. It’s a solution designed to scale with your ambitions, ensuring a secure and future-ready application environment.</p>
<p>💡 If you’re looking to leverage Keycloak for your organization or need guidance in your implementation journey, feel free to connect!</p>
<p>👉 <strong>Connect with me on LinkedIn:</strong> <a href="https://www.linkedin.com/in/deep-modak-10b914244/">Deep Modak</a></p>
