# Test Cases: VWO Login Dashboard

These test cases are derived from the Product Requirements Document (PRD) for the VWO Login Dashboard (app.vwo.com).

## 1. UI & Visual Design Tests
| Test Case ID | Test Scenario | Steps | Expected Result |
| :--- | :--- | :--- | :--- |
| **TC_UI_001** | Verify default login page layout | 1. Navigate to app.vwo.com | Login form with VWO branding is displayed cleanly. "Email Address", "Password", "Remember Me", and a "Registration/Free Trial" link are visible. |
| **TC_UI_002** | Verify Auto-focus | 1. Navigate to the login page | The "Email Address" field should automatically receive focus upon page load. |
| **TC_UI_003** | Verify responsive design on mobile | 1. Open login page on mobile device simulation | Layout should be touch-friendly, properly scaled, and optimized for mobile screens without horizontal scrolling. |
| **TC_UI_004** | Verify Light and Dark Theme Options | 1. Toggle between Light and Dark Mode (if available on banner) | The UI should successfully transition its color schemes and maintain VWO brand consistency. |
| **TC_UI_005** | Verify Accessibility (Keyboard Navigation) | 1. Use the 'Tab' key to navigate | All interactive elements (inputs, checkboxes, links, buttons) are sequentially reachable via keyboard navigation. |

## 2. Authentication (Valid) Tests
| Test Case ID | Test Scenario | Steps | Expected Result |
| :--- | :--- | :--- | :--- |
| **TC_AUTH_001** | Successful Login with valid credentials | 1. Enter valid Email Address<br>2. Enter correct Password<br>3. Click "Sign In" | User successfully logs in, transitions seamlessly to the VWO core platform dashboard. |
| **TC_AUTH_002** | Verify 'Remember Me' Functionality | 1. Enter valid credentials<br>2. Check "Remember Me"<br>3. Click "Sign In"<br>4. Close browser and reopen app.vwo.com | User is automatically authenticated without needing to re-enter credentials. |
| **TC_AUTH_003** | Verify 2FA / MFA Support | 1. Login with a valid 2FA enabled account<br>2. Enter email/password<br>3. Enter OTP | User is prompted for the OTP and gets successfully authenticated ONLY after providing the valid OTP. |
| **TC_AUTH_004** | Login Success tracking (Analytics Integration) | 1. Perform successful login | An analytics event tracking the successful login should be fired. |

## 3. Authentication (Invalid & Error Handling) Tests
| Test Case ID | Test Scenario | Steps | Expected Result |
| :--- | :--- | :--- | :--- |
| **TC_ERR_001** | Invalid Email Format (Real-time Validation) | 1. Enter "user@invalid"<br>2. Click outside the field (blur) | Immediate validation error should prompt the user about an invalid email format. |
| **TC_ERR_002** | Login with unregistered email | 1. Enter email not in system<br>2. Enter any password<br>3. Click "Sign In" | Clear and actionable error message indicating authentication failure. |
| **TC_ERR_003** | Login with incorrect password | 1. Enter valid email<br>2. Enter incorrect password<br>3. Click "Sign In" | Clear and actionable error message indicating authentication failure. |
| **TC_ERR_004** | Empty credentials submission | 1. Leave email and password blank<br>2. Click "Sign In" | Warning/Error validation text should require both fields to be filled. |
| **TC_ERR_005** | Rate-limiting (Brute force protection) | 1. Attempt >10 invalid logins rapidly | Request throttling occurs; error message stating too many attempts (or captcha appears) blocking further attempts. |

## 4. Advanced Negative & Edge Cases
| Test Case ID | Test Scenario | Steps | Expected Result |
| :--- | :--- | :--- | :--- |
| **TC_NEG_001** | SQL Injection Attempt in Inputs | 1. Enter `' OR 1=1 --` in Email and Password<br>2. Click "Sign In" | Application must sanitize input. Validation error occurs or authentication fails securely. No DB exposure or successful login. |
| **TC_NEG_002** | XSS Script in Email Field | 1. Enter `<script>alert('xss')</script>`<br>2. Click "Sign In" | Application sanitizes input and does not execute the script. Fails with authentication error. |
| **TC_NEG_003** | Leading/Trailing Spaces in Password | 1. Enter valid email<br>2. Enter valid password but add space at the end<br>3. Click "Sign In" | Authentication should fail, password hashes will not match. |
| **TC_NEG_004** | Browser Back Button after Logout | 1. Login successfully<br>2. Logout<br>3. Click browser 'Back' button | User should not access the dashboard and should be redirected to the login screen. |
| **TC_NEG_005** | Extremely Long Inputs | 1. Enter 500+ characters in Email/Password fields<br>2. Click "Sign In" | Max length validation should restrict input, or appropriate error is shown without crashing the application. |
| **TC_NEG_006** | Copy/Paste restrictions | 1. Right-click or use Ctrl+V on Password field | Behavior should follow security standards (often allowing paste for password managers, but worth verifying against PRD strictly). |

## 5. Password Management Tests
| Test Case ID | Test Scenario | Steps | Expected Result |
| :--- | :--- | :--- | :--- |
| **TC_PWD_001** | Verify Forgot Password link | 1. Click on the 'Forgot Password' link | User is redirected to a streamlined password recovery page. |
| **TC_PWD_002** | Request password reset securely | 1. Enter valid email in recovery page<br>2. Submit | A secure password reset token/link is generated and sent to the respective email address. |
| **TC_PWD_003** | Enforce password security standards | 1. Attempt to reset password with weak string (e.g., "password123") | Visual feedback indicators show password requirements (strength). System rejects the weak password. |
| **TC_PWD_004** | Expired Reset Link | 1. Click a password reset link older than configured expiry | Link should be invalid with an appropriate expiration message. |

## 6. Enterprise & Third-Party Integrations
| Test Case ID | Test Scenario | Steps | Expected Result |
| :--- | :--- | :--- | :--- |
| **TC_INT_001** | Single Sign-On (SSO) login attempt | 1. Click "Sign in with SSO" (if applicable)<br>2. Authenticate through identity provider | User is redirected through SAML/OAuth flow and successfully logs into the VWO dashboard. |
| **TC_INT_002** | Social Login via Google/Microsoft | 1. Click "Sign in with Google"<br>2. Complete Google Auth flow | User successfully logs in; marketing and customer onboarding analytics are integrated. |

## 7. Performance & Security Tests
| Test Case ID | Test Scenario | Steps | Expected Result |
| :--- | :--- | :--- | :--- |
| **TC_PERF_001** | Verify Page Load Time | 1. Navigate to app.vwo.com on standard 4G/WiFi | Page fully loads in under 2 seconds. |
| **TC_SEC_001** | Verify HTTPS enforcement | 1. Attempt to access login page via `http://app.vwo.com` | Request is automatically redirected to `https://app.vwo.com` with valid SSL/TLS certificate. |
| **TC_SEC_002** | Secure Session Handling | 1. Successfully log in<br>2. Leave dashboard idle for timeout period | Session automatically expires, protecting user data. |
