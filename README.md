# Case-Study-Report-Sab'a
## Group Member Details 
| No.| Names                     | Matric No. |
|----|:-------------:            | :---------------:|
| 1 | AMEER AL-WAFIQ BIN NORAZAM | 2119005 |
| 2 | SALSABILA TASYA HARRIS     | 1914606 |
| 3 | ROBBANI GHOZI FIKRI        | 1832765 |
| 4 | ABDUL RASHID BIN NUHAIRI   | 1911767 |
| 5 |                            ||
## Table of Contents
| No.| Alerts                     | Person in Charge |
|----|:-------------:              | :---------------:|
| 1 | CSRF Attack                 | Tasya |
| 2 | CSP                         | Ghozi |
| 3 | JS Library                  | Ameer |
| 4 | Information Disclosure      | Rahat |
| 5 | Potential XSS               | Rashid |

## Brief Description
- 
## Objectives
- 
### 1. CSRF attack
>>>>>>> a3cf62c766ce6da1b19effb663a257d472340230
CWE ID : 352

CSRF stands for Cross Site Request Forgery. CSRF is a type of security vulnerability that can occur in web application when an attacker creates a malicious page that make a request to the server on behalf of the victim's authentication or session identification cookie. Cross-site request forgery is the practise of tricking a user's browser into sending a request to a target web app on their behalf. The web application's trust in the user's browser is exploited by the attack. The user can be tricked into visiting a malicious website or clicking on a specifically created link, which the attacker can use to carry out unauthorised actions on the targeted web application.

Based on the result after scan the vulnerabilities on OWASP, this website has 314 absences of anti-CSRF tokens on the HTML form.

One of the evidence is 
```HTML
<form action="https://kulliyyah.iium.edu.my/ahaskirkhs/wp-comments-post.php" method="post" id="commentform" class="comment-form">
```

In order to prevent CSRF attack, we can implement the anti - CSRF token. CSRF token is a unique value that is generated by the server and sent to the client as a hidden input field in the form. When the user submits the form, the value of token is also sent back to the server. The server then checks to make sure that the value of token matches what it expects. If the value dont match, the server knows that the request was not initiated bu the actual user and can reject it.  The risk level of this threat is medium, which means this security issue has potential to cause moderate harm to the system. 

Implementing anti-CSRF token: 
 
In server side : php
```PHP
<?php
// Generate and store CSRF token in the user's session
session_start();
$csrfToken = bin2hex(random_bytes(32)); // Generate a random token
$_SESSION['csrf_token'] = $csrfToken; // Store the token in the session
?>
```

In code above, the CSRF token generated using secure random value generation function randoom_bytes(). Then the generated token is stored in the user's session $_SESSION['csrf_token'] for later validation.

In HTML form 

```HTML
<form action="https://kulliyyah.iium.edu.my/ahaskirkhs/wp-comments-post.php" method="post" id="commentform" class="comment-form">
  <!-- Include the CSRF token as a hidden input field -->
  <input type="hidden" name="csrf_token" value="<?php echo $csrfToken; ?>">
  
  <!-- Other form fields -->
  <!-- ... -->
  
  <!-- Submit button -->
  <input type="submit" value="Submit">
</form>
```

In HTML, the CSRF token included as hidden input field (  ```HTML <input type="hidden" >```). The name attribute of the input field (name="csrf_token") is to identify when the form is submitted. The value attribute (value="<?php echo $csrfToken; ?>") is to dynamically insert token into HTML. 

The CSRF token will be part of the form data when it is submitted. To confirm the validity of the form submission and guard against CSRF attacks, server-side validation of the submitted token against the stored token in the user's session is an option.

Another way to mitigate the CSRF attack is to apply the double submitting cookies, here two various ways exist for the CSRF token to be transmitted: as a cookie (which is automatically sent with every request) and as a hidden field in the form (which is manually submitted with the request by the user submitting the form). The server may confirm that the request was performed on purpose by the user from the same origin by comparing the two values.

It will be challenging to match the CSRF token values if an attacker tries to fake a request by submitting a form from a different website because they won't have access to the user's CSRF token cookie.

In addition, preventing the XSS first could help to prevent CSRF. Enabling user interation whenever user needs to perform update to the sensitive data also able to mitigate the CSRF attacks because the attacker still needs to get the user's actual password. This could be done by implementing reauthentication mechanisms, CAPTCHA challenges, and one time token. 

* REFERENCES

Cross Site Request Forgery (CSRF). (n.d.). OWASP Foundation. Retrieved May 7, 2023, 
    from https://owasp.org/www-community/attacks/csrf
CSRF Attacks: Anatomy, Prevention, and XSRF Tokens. (n.d.). Acunetix. Retrieved May 7, 2023, 
    from https://www.acunetix.com/websitesecurity/csrf-attacks/
6 CSRF Mitigation Techniques You Must Know. (2022, May 4). Bright Security. Retrieved May 7, 2023,
    from https://brightsec.com/blog/csrf-mitigation/#double-submitting-cookies
    
    
### 2. CSP
- (Ghozi)
### 3. JS Library
- (Ameer)
### 4. Information Disclosure
- (Rahat)
### 5. Potential XSS
- (Rashid)
