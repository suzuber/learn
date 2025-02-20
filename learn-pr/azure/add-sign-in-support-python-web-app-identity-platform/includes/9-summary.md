
You're developing a web application that signs in users and allows them to access data based on their roles in the company. The application you built uses the Microsoft identity platform to enforce authentication and provide access control.

Adding authentication enables your web app to access limited profile information and customize the experience offered to different users. In this scenario, the web app authenticates users in a web browser by redirecting them to sign in to Microsoft identity. Microsoft identity then returns a sign-in response through the user’s browser, which contains claims about the user in an identity token.

You also used Role Based Access Control (RBAC) to enforce authorization in the Flask web application.To implement the app role business logic in this scenario, you defined the app roles in **App registrations**. Then, you assigned the roles to different users in the **Enterprise applications** pane. The assigned app roles are included in any token that's issued for your application, either ID tokens when the app is signing in a user or access tokens when the app is calling a protected web API. By using RBAC, you securely control who has access to what content and what functionality in your application.

While it's possible to build an application without delegating authentication and authorization to a centralized identity provider like Microsoft Entra ID, it would bring a high administrative burden to the developer. You'd need to create an application that maintains all username and password information and find a mechanism for adding, removing, and adjusting user permissions across multiple apps. As the number of users increase, and your application grows in scale and complexity, app roles become even more useful.

## References

* [Microsoft identity platform web app documentation](/azure/active-directory/develop/index-web-app)
* [Microsoft identity platform and OAuth 2.0 authorization code flow](/azure/active-directory/develop/v2-oauth2-auth-code-flow)
* [Using app roles to enforce role-based access control](/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps)
