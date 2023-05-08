# Case-Study-Report-Sab'a
## Group Member Details 
| No.| Names                     | Matric No. |
|----|:-------------:            | :---------------:|
| 1 | AMEER AL-WAFIQ BIN NORAZAM | 2119005 |
| 2 | SALSABILA TASYA HARRIS     | 1914606 |
| 3 | ROBBANI GHOZI FIKRI        | 1832765 |
| 4 | ABDUL RASHID BIN NUHAIRI   | 1911767 |
| 5 | MD MOSTAFIZUR RAHMAN RAHAT | 1823811 |

## Table of Contents
| No. | Sub| Content                                                                                                    | Person in Charge |
|-----|:--:|:-------------:                                                                                             | :---------------:|
| 1.0 | -- |[Brief Description ](https://github.com/ameeralwafiq/Case-Study-Report-Sab-a#10-brief-description)          | All |
| 2.0 | -- |[Objective](https://github.com/ameeralwafiq/Case-Study-Report-Sab-a#20-objectives)                          | All |
| 3.0 | -- |[Alerts](https://github.com/ameeralwafiq/Case-Study-Report-Sab-a/#30-alerts)                                 | All |
| -- | 3.1 |[CSRF Attack](https://github.com/ameeralwafiq/Case-Study-Report-Sab-a/#31-csrf-attack)                     | Tasya |
| -- | 3.2 |[CSP](https://github.com/ameeralwafiq/Case-Study-Report-Sab-a/#32-csp)                                     | Ghozi |
| -- | 3.3 |[JS Library](https://github.com/ameeralwafiq/Case-Study-Report-Sab-a/#33-js-library)                       | Ameer |
| -- | 3.4 |[Information Disclosure](https://github.com/ameeralwafiq/Case-Study-Report-Sab-a/#34-information-disclosure) | Rahat |
| -- | 3.5 |[Potential XSS](https://github.com/ameeralwafiq/Case-Study-Report-Sab-a/#35-potential-xss)                 | Rashid |


## 1.0 Brief Description
- 
In this case study, https://kulliyyah.iium.edu.my/koe/#  its web application vulnerability will be evaluated. This website is serves a platform to share wide range of information and resources about engineering faculty in IIUM. This website serves as virtual gateways to the institution, providing a range of features and sections that satisfy the requirements of different users. Generally, this website gives an overview of Kuliyyah of Engineering IIUM faculty, including information on its vision, mission, and building layout. The profile of faculty officers and staff also included, with the purpose of show their credentials, areas of interest om research, as well as their contact details. Visitors are kept up to date on the most recent accomplishments, advancements, and future events within the academic community through the news and events pages.  

## 2.0 Objectives
- The objectives of this case study is to identify the vulnerabilities that exists in the web application https://kulliyyah.iium.edu.my/koe/, evaluate the vulnerabilities of the web application and find the ways to prevent and mitigate the vulnerabilities of the web application...
- 

## 3.0 Alerts
### 3.1 CSRF attack
>>>>> a3cf62c766ce6da1b19effb663a257d472340230
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
    
    
### 3.2 CSP
- (Ghozi)
>>>>> Passive (10038 - Content Security Policy (CSP) Header Not Set)
CWE ID : 693

Content Security Policy (CSP) is an added layer of security that helps to detect and mitigate certain types of attacks, including Cross-Site Scripting (XSS) and data injection attacks. These attacks are used for everything from data theft, to site defacement, to malware distribution.

Based on the result after scan the vulnerabilities on OWASP, this website has 314 absences of anti-CSRF tokens on the HTML form.

One of the evidence is

### 3.3 JS Library
- (Ameer Al-Wafiq bin Norazam 2119005)
>>>>>>> Passive (10003 - Vulnerable JS Library (Powered by Retire.js))
CWE ID : 829

JavaScript Library or JS Library is a ... Meanwhile, Vulnerable JS Library can be said when a JS Library used in the web application is vulnerable to attacks from the outside.

Vulnerable JS Lirary of a web application can be exploited by attackes...
Vulnerable JS Library are vulnerable because of...

### 3.4 Information Disclosure
- (Rahat)
- 
### 3.5 Potential XSS
- (Abdul Rashid bin Nuhairi - 1911767)
>>>>> Passive (10031 - User Controllable HTML Element Attribute (Potential XSS))
CWE ID : 20

Here are the results from OWASP ZAP that has been noted and its brief.

**Description:** User-controllable HTML element attribute (potential XSS) is a type of vulnerability that can exist in websites, including those built using Wordpress. This vulnerability occurs when a website allows users to control certain attributes of HTML elements, such as the "src" attribute of an image tag or the "href" attribute of a link tag, without properly validating or sanitizing the input.

**Risk: informational** - "Risk: informational" means that the vulnerability identified by the tool does not pose an immediate threat or allow for direct exploitation, but provides information that could be used by an attacker to further exploit vulnerabilities or launch a more sophisticated attack. These types of vulnerabilities do not usually require immediate remediation, but should be addressed as part of an overall security program.

**Confidence: low** - "Confidence: low" means that the tool is not completely certain that the identified vulnerability exists. The confidence level is based on the reliability and accuracy of the tool's detection mechanisms, and is influenced by various factors such as the complexity of the application, the depth of the scan, and the level of interaction with the target.

**Source: passive (10031)** - "Source: passive (10031)" refers to the method used by the tool to detect the vulnerability. "Passive" means that the tool is monitoring network traffic and analyzing responses from the target, without actually sending any payloads or inputs that could affect the application. "10031" is a unique identifier for the specific plugin or rule that detected the vulnerability. In this case, it could refer to a plugin or rule related to information disclosure or sensitive data leakage.

**CWE ID: 20** refers to the Common Weakness Enumeration (CWE) identifier for the vulnerability category "Improper Input Validation." CWE is a community-developed dictionary of common software security weaknesses, which provides a standardized language for describing security vulnerabilities in software and hardware.
Specifically, CWE ID: 20 refers to cases where input from a user or an external system is not properly validated, allowing an attacker to inject malicious data into the system or execute unintended actions.

**WASC ID: 20** refers to the Web Application Security Consortium (WASC) identifier for the vulnerability category "Improper Input Handling." WASC is a group of international experts in web application security, who have created a set of guidelines and best practices for securing web applications.

**Prevention:** To prevent user-controllable HTML element attribute vulnerabilities and potential XSS attacks, it's important to properly validate and sanitize all user input on the server-side, and to use secure coding practices when developing websites and web applications. Additionally, websites should use Content Security Policy (CSP) headers to restrict the types of content that can be loaded on their pages, which can help mitigate the impact of XSS attacks. . Additionally, WordPress provides several built-in functions and plugins that can be used to sanitize and validate user input, but it's up to website developers to implement them correctly. 
