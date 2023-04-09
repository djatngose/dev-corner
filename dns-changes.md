# 
When changing the domain of a live project, there are several things to consider to ensure that the system does not break:

DNS settings: Ensure that the new domain's DNS settings are properly configured to point to the correct IP address of the server where the project is hosted. You may need to update the A record or CNAME record of the domain.

SSL certificate: If you are using SSL to secure your website, you will need to ensure that your SSL certificate is valid for the new domain name. You may need to update or obtain a new SSL certificate.

URL redirection: Set up URL redirection to redirect all the requests made to the old domain to the new domain. This can be done using a .htaccess file or through server-side scripting.

Hardcoded links: Check the project's codebase for any hardcoded links that may reference the old domain name. Update these links to reflect the new domain name.

Testing: Thoroughly test the project after the domain change to ensure that everything is working correctly. Test all functionality, including user authentication, payment processing, and any other critical features.

Communication: Inform all stakeholders, such as users and partners, about the domain change and provide clear instructions on how to access the project from the new domain.

Checking IP caching TTL
align with devops
review code and config if they are hardcoded paths
perform analysis with other services
prepare test plan on dev/test systems
