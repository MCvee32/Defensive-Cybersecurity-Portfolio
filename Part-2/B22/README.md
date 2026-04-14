# Part 2 - B22: Enhance the cybersecurity of a website from your community.

### Target Website: https://sorrentotennisclub.com.au
**Website Vulnerability:** WordPress admin login URL publicly linked in footer  
CWE-200 Exposure of sensitive information to an unauthorised actor   
<img width="1440" height="574" alt="Screenshot 2026-04-14 at 1 51 10 pm" src="https://github.com/user-attachments/assets/2b8b4bc7-a203-416f-ada9-79b39cb5b0e3" />  
Every page on the site contains a visible hyperlink labelled "Admin" in the footer that navigates directly to /wp-admin. This "advertises" the admin login panel to every visitor, search engine crawler, and automated scanner. Reducing the effort required to launch a brute-force or credential stuffing attack.

**Risk Explanation:** The website is powered by WordPress, so /wp-admin is the first path automated attack tools probe on any site. Without the footer link, an attacker still has to guess the path. With the footer link, it is accessible in one-click. Combined with the confirmed username admin visible in author URLs on the same site, an attacker has both the login URL and a valid username, leaving only the password to guess. A typical credential stuffing attack or brute-force attack can give an attacker unauthorised access.

### Enhancement
1. Remove the admin hyperlink from the theme footer
- Removing this anchor element entirely is important, and no functionality is lost becuase the WordPress admin is still accessible by typing the URL directly when needed.  

2. Relocate the Login Path  
- Move /wp-admin to a custom URL  
Can be performed using WPS Hide Login. This plugin intercepts requests to /wp-admin and /wp-login.php and redirects them to a 404 page, while serving the real login form at a custom path.
Method:
1. Install WPS Hide Login plugin and Activate it
2. Set the login URL to stclub-access (this is non-obvious, unlike "admin" or "login")

New Custom Path: https://sorrentotennisclub.com.au/stclub-access  
With this fix when the website is visted there is no admin link present, and /wp-admin and /wp-login.php attempts will lead to 404 pages. This enhances the webiste's security against automated scanning tools and brute-force attacks.

