---
layout: page
title: Login
permalink: /login
search_exclude: true
show_reading_time: false
---
<br>

<script src="https://accounts.google.com/gsi/client" async defer></script>

<style>
.auth-wrapper {
    display: flex;
    gap: 2rem;
    max-width: 900px;
    margin: 0 auto;
    flex-wrap: wrap;
}
.auth-card {
    flex: 1;
    min-width: 280px;
    background: #1e1e2e;
    border: 1px solid #3b3b5c;
    border-radius: 12px;
    padding: 2rem;
}
.auth-card h2 { margin: 0 0 1.5rem; font-size: 1.4rem; color: #e2e8f0; }
.auth-card hr { border-color: #3b3b5c; margin-bottom: 1.5rem; }
.form-group { margin-bottom: 1rem; }
.form-group input, .form-group select {
    width: 100%;
    padding: 0.65rem 0.9rem;
    background: #12121f;
    border: 1px solid #3b3b5c;
    border-radius: 8px;
    color: #e2e8f0;
    font-size: 0.95rem;
    box-sizing: border-box;
}
.form-group input:focus, .form-group select:focus {
    outline: none; border-color: #6366f1;
}
.btn-primary {
    width: 100%; padding: 0.7rem;
    background: #6366f1; color: white;
    border: none; border-radius: 8px;
    font-size: 1rem; cursor: pointer; margin-top: 0.25rem;
}
.btn-primary:hover { background: #4f46e5; }
.btn-primary:disabled { background: #4b4b6b; cursor: not-allowed; }
.btn-secondary {
    width: 100%; padding: 0.7rem;
    background: transparent; color: #a0aec0;
    border: 1px solid #3b3b5c; border-radius: 8px;
    font-size: 0.9rem; cursor: pointer; margin-top: 0.5rem;
}
.btn-secondary:hover { border-color: #6366f1; color: #e2e8f0; }
.divider {
    display: flex; align-items: center;
    gap: 0.75rem; margin: 1.25rem 0;
    color: #6b7280; font-size: 0.85rem;
}
.divider::before, .divider::after {
    content: ''; flex: 1; height: 1px; background: #3b3b5c;
}
.status-msg {
    margin-top: 0.75rem; padding: 0.6rem 0.9rem;
    border-radius: 8px; font-size: 0.9rem; display: none;
}
.status-msg.show { display: block; }
.status-msg.success { background: #14532d; color: #86efac; }
.status-msg.error { background: #450a0a; color: #fca5a5; }
.status-msg.info { background: #1e3a5f; color: #93c5fd; }
.otp-step { display: none; margin-top: 0.5rem; }
.otp-step.show { display: block; }
.otp-input { letter-spacing: 0.4rem; text-align: center; font-size: 1.4rem; }
.validation-message { font-size: 0.8rem; margin-top: 0.3rem; }
.validation-message.success { color: #86efac; }
.validation-message.error { color: #fca5a5; }
.overall-status {
    margin-top: 1rem; padding: 0.6rem 0.9rem;
    border-radius: 8px; font-size: 0.9rem;
}
.overall-status.hidden { display: none; }
.overall-status.success { background: #14532d; color: #86efac; }
.overall-status.error { background: #450a0a; color: #fca5a5; }
.signup-toggle {
    text-align: center; margin-top: 1rem;
    color: #9ca3af; font-size: 0.9rem;
}
.signup-toggle a { color: #818cf8; cursor: pointer; text-decoration: underline; }
.signup-section { display: none; }
.signup-section.show { display: block; }
.oauth-box { display: none; text-align: center; }
.oauth-box.show { display: block; }
.oauth-status { margin-top: 0.75rem; font-size: 0.9rem; }
.backend-status { display: flex; gap: 0.5rem; margin-top: 0.75rem; }
.status-item { font-size: 0.85rem; color: #9ca3af; }
</style>

<div class="auth-wrapper">

    <!-- LOGIN CARD -->
    <div class="auth-card">
        <h2>Login</h2>
        <hr>

        <!-- Step 1: credentials -->
        <div id="loginStep1">
            <div class="form-group">
                <input type="email" id="loginEmail" placeholder="Email address" autocomplete="email">
            </div>
            <div class="form-group">
                <input type="password" id="loginPassword" placeholder="Password" autocomplete="current-password">
            </div>
            <button class="btn-primary" id="sendOtpBtn" onclick="sendOtp()">Send Verification Code</button>
            <div id="loginMsg" class="status-msg"></div>

            <div class="divider">or</div>

            <div id="google-login-onload"
                 data-client_id="714327350398-q7jtd45cknoa0ijsgsg0d0iedk7epqdo.apps.googleusercontent.com"
                 data-callback="handleGoogleLogin"
                 data-auto_prompt="false">
            </div>
            <div class="g_id_signin"
                 data-type="standard"
                 data-size="large"
                 data-theme="filled_blue"
                 data-text="signin_with"
                 data-shape="rectangular"
                 data-logo_alignment="left"
                 data-context="signin">
            </div>
        </div>

        <!-- Step 2: OTP entry -->
        <div id="loginStep2" class="otp-step">
            <p style="color:#9ca3af; font-size:0.9rem; margin-bottom:1rem;">
                Enter the 6-digit code sent to
                <strong id="otpEmailLabel" style="color:#e2e8f0;"></strong>
            </p>
            <div class="form-group">
                <input type="text" id="otpCode" class="otp-input"
                       placeholder="000000" maxlength="6"
                       inputmode="numeric" autocomplete="one-time-code">
            </div>
            <button class="btn-primary" onclick="verifyOtp()">Verify &amp; Login</button>
            <button class="btn-secondary" onclick="backToLoginStep1()">Back</button>
            <div id="otpMsg" class="status-msg"></div>
        </div>

        <div class="signup-toggle">
            Don't have an account? <a onclick="toggleSignup()">Sign up</a>
        </div>
    </div>

    <!-- SIGNUP CARD -->
    <div class="auth-card signup-section" id="signupCard">
        <h2>Sign Up</h2>
        <hr>

        <!-- Verification step (shown after form submit) -->
        <div id="oauth-verification" class="oauth-box">
            <h3 style="color:#6366f1; margin-bottom:0.75rem; font-size:1.1rem;">
                Verify Your Email
            </h3>
            <p style="color:#9ca3af; font-size:0.9rem; margin-bottom:1.25rem;">
                Verify your identity to create your account.
            </p>

            <!-- Option 1: Email OTP -->
            <div id="signupOtpStep1">
                <button class="btn-primary" onclick="sendSignupOtp()">
                    Send Code to <span id="signupEmailLabel" style="font-style:italic;"></span>
                </button>
                <div id="signupOtpMsg" class="status-msg" style="margin-top:0.5rem;"></div>
            </div>
            <div id="signupOtpStep2" style="display:none; margin-top:0.75rem;">
                <div class="form-group">
                    <input type="text" id="signupOtpCode" class="otp-input"
                           placeholder="000000" maxlength="6"
                           inputmode="numeric" autocomplete="one-time-code">
                </div>
                <button class="btn-primary" onclick="verifySignupOtp()">Verify &amp; Create Account</button>
                <button class="btn-secondary" onclick="resetSignupOtp()">Resend Code</button>
                <div id="signupOtpVerifyMsg" class="status-msg" style="margin-top:0.5rem;"></div>
            </div>

            <div class="divider">or</div>

            <!-- Option 2: Google -->
            <div id="g_id_onload"
                 data-client_id="714327350398-q7jtd45cknoa0ijsgsg0d0iedk7epqdo.apps.googleusercontent.com"
                 data-callback="handleGoogleSignIn"
                 data-auto_prompt="false">
            </div>
            <div class="g_id_signin"
                 data-type="standard"
                 data-size="large"
                 data-theme="filled_blue"
                 data-text="signin_with"
                 data-shape="rectangular"
                 data-logo_alignment="left">
            </div>
            <div id="oauth-status" class="oauth-status"></div>

            <button class="btn-secondary" onclick="showSignupForm()" style="margin-top:0.75rem;">
                Back to Form
            </button>
        </div>

        <!-- Signup form -->
        <form id="signupForm" onsubmit="handleSignupSubmit(event);">
            <div class="form-group">
                <input type="text" id="name" placeholder="Full Name" required>
            </div>
            <div class="form-group">
                <input type="text" id="signupUid" placeholder="User ID" required>
            </div>
            <div class="form-group">
                <input type="text" id="signupSid" placeholder="Student ID" required>
            </div>
            <div class="form-group">
                <select id="signupSchool" required>
                    <option value="" disabled selected>Select Your High School</option>
                    <option value="Abraxas High School">Abraxas</option>
                    <option value="Del Norte High School">Del Norte</option>
                    <option value="Mt Carmel High School">Mt Carmel</option>
                    <option value="Poway High School">Poway</option>
                    <option value="Poway to Palomar">Poway to Palomar</option>
                    <option value="Rancho Bernardo High School">Rancho Bernardo</option>
                    <option value="Westview High School">Westview</option>
                </select>
            </div>
            <div class="form-group">
                <input type="email" id="signupEmail" placeholder="Email address" required>
            </div>
            <div class="form-group">
                <input type="password" id="signupPassword" placeholder="Password (min 8 chars)" required>
            </div>
            <div class="form-group">
                <input type="password" id="confirmPassword" placeholder="Confirm Password" required>
                <div id="password-validation-message" class="validation-message"></div>
            </div>
            <label style="display:flex; align-items:center; gap:0.5rem; color:#9ca3af; font-size:0.85rem; margin-bottom:1rem; cursor:pointer;">
                <input type="checkbox" id="kasmNeeded"> Kasm Server Needed
            </label>
            <button type="submit" class="btn-primary">Create Account</button>
            <div class="backend-status">
                <div id="flaskStatus" class="status-item">
                    <span class="status-icon">&#8987;</span>
                    <span class="status-text">Flask</span>
                </div>
            </div>
            <div id="overallStatus" class="overall-status hidden"></div>
        </form>

        <div class="signup-toggle">
            Already have an account? <a onclick="toggleSignup()">Log in</a>
        </div>
    </div>

</div>

<p id="message" style="display:none;"></p>

<script type="module">
    import { pythonURI } from '{{site.baseurl}}/assets/js/api/config.js';

    const GOOGLE_CLIENT_ID = "714327350398-q7jtd45cknoa0ijsgsg0d0iedk7epqdo.apps.googleusercontent.com";
    let signupFormData = {};
    let verifiedSchoolEmail = null;
    let validationTimeout = null;

    // ── Utilities ──────────────────────────────────────────────────────────────

    function showMsg(id, text, type) {
        const el = document.getElementById(id);
        el.textContent = text;
        el.className = `status-msg show ${type}`;
    }

    function redirect() {
        setTimeout(() => { window.location.href = '{{site.baseurl}}/profile'; }, 800);
    }

    // ── OTP Login ──────────────────────────────────────────────────────────────

    window.sendOtp = async function() {
        const email    = document.getElementById('loginEmail').value.trim();
        const password = document.getElementById('loginPassword').value;
        if (!email || !password) {
            showMsg('loginMsg', 'Please enter your email and password.', 'error');
            return;
        }
        const btn = document.getElementById('sendOtpBtn');
        btn.disabled = true;
        btn.textContent = 'Sending...';
        try {
            const res  = await fetch(`${pythonURI}/api/otp/send`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ email, password })
            });
            const data = await res.json();
            if (res.ok) {
                document.getElementById('loginStep1').style.display = 'none';
                document.getElementById('loginStep2').classList.add('show');
                document.getElementById('otpEmailLabel').textContent = email;
                showMsg('otpMsg', data.message, 'info');
            } else {
                showMsg('loginMsg', data.message || 'Failed to send code.', 'error');
                btn.disabled = false;
                btn.textContent = 'Send Verification Code';
            }
        } catch (e) {
            showMsg('loginMsg', 'Network error. Is the backend running?', 'error');
            btn.disabled = false;
            btn.textContent = 'Send Verification Code';
        }
    };

    window.verifyOtp = async function() {
        const email = document.getElementById('loginEmail').value.trim();
        const otp   = document.getElementById('otpCode').value.trim();
        if (!otp) { showMsg('otpMsg', 'Enter the verification code.', 'error'); return; }
        showMsg('otpMsg', 'Verifying...', 'info');
        try {
            const res  = await fetch(`${pythonURI}/api/otp/verify`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                credentials: 'include',
                body: JSON.stringify({ email, otp })
            });
            const data = await res.json();
            if (res.ok) {
                showMsg('otpMsg', 'Verified! Redirecting...', 'success');
                redirect();
            } else {
                showMsg('otpMsg', data.message || 'Invalid code.', 'error');
            }
        } catch (e) {
            showMsg('otpMsg', 'Network error.', 'error');
        }
    };

    window.backToLoginStep1 = function() {
        document.getElementById('loginStep2').classList.remove('show');
        document.getElementById('loginStep1').style.display = '';
        const btn = document.getElementById('sendOtpBtn');
        btn.disabled = false;
        btn.textContent = 'Send Verification Code';
        document.getElementById('otpCode').value = '';
    };

    // ── Google Login ───────────────────────────────────────────────────────────

    window.handleGoogleLogin = async function(response) {
        showMsg('loginMsg', 'Signing in with Google...', 'info');
        try {
            const res  = await fetch(`${pythonURI}/api/google/login`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                credentials: 'include',
                body: JSON.stringify({ credential: response.credential })
            });
            const data = await res.json();
            if (res.ok) {
                showMsg('loginMsg', 'Signed in! Redirecting...', 'success');
                redirect();
            } else {
                showMsg('loginMsg', data.message || 'Google login failed. Make sure you have an account.', 'error');
            }
        } catch (e) {
            showMsg('loginMsg', 'Network error.', 'error');
        }
    };

    // ── Signup toggle ──────────────────────────────────────────────────────────

    window.toggleSignup = function() {
        document.getElementById('signupCard').classList.toggle('show');
    };

    // ── Signup ─────────────────────────────────────────────────────────────────

    function validatePasswordsDebounced() {
        clearTimeout(validationTimeout);
        validationTimeout = setTimeout(() => {
            const pw  = document.getElementById('signupPassword').value;
            const cpw = document.getElementById('confirmPassword').value;
            const msgEl = document.getElementById('password-validation-message');
            const field = document.getElementById('confirmPassword');
            field.classList.remove('password-match', 'password-mismatch');
            msgEl.className = 'validation-message';
            if (!cpw) { msgEl.textContent = ''; return; }
            if (pw.length < 8) {
                msgEl.classList.add('error');
                msgEl.textContent = 'Password must be at least 8 characters';
                return;
            }
            if (pw === cpw) {
                field.classList.add('password-match');
                msgEl.classList.add('success');
                msgEl.textContent = 'Passwords match';
            } else {
                field.classList.add('password-mismatch');
                msgEl.classList.add('error');
                msgEl.textContent = 'Passwords do not match';
            }
        }, 800);
    }

    window.handleSignupSubmit = function(event) {
        event.preventDefault();
        const pw  = document.getElementById('signupPassword').value;
        const cpw = document.getElementById('confirmPassword').value;
        if (pw !== cpw)  { alert('Passwords do not match.'); return; }
        if (pw.length < 8) { alert('Password must be at least 8 characters.'); return; }
        signupFormData = {
            name:              document.getElementById('name').value,
            uid:               document.getElementById('signupUid').value,
            sid:               document.getElementById('signupSid').value,
            school:            document.getElementById('signupSchool').value,
            email:             document.getElementById('signupEmail').value,
            password:          pw,
            kasm_server_needed: document.getElementById('kasmNeeded').checked,
        };
        document.getElementById('signupEmailLabel').textContent = signupFormData.email;
        document.getElementById('signupForm').style.display = 'none';
        document.getElementById('oauth-verification').classList.add('show');
    };

    window.showSignupForm = function() {
        document.getElementById('oauth-verification').classList.remove('show');
        document.getElementById('signupForm').style.display = '';
        document.getElementById('oauth-status').textContent = '';
    };

    // ── Signup OTP ─────────────────────────────────────────────────────────────

    window.sendSignupOtp = async function() {
        const email = signupFormData.email;
        if (!email) { return; }
        showMsg('signupOtpMsg', 'Sending code...', 'info');
        try {
            const res  = await fetch(`${pythonURI}/api/otp/signup/send`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ email })
            });
            const data = await res.json();
            if (res.ok) {
                showMsg('signupOtpMsg', data.message, 'success');
                document.getElementById('signupOtpStep1').style.display = 'none';
                document.getElementById('signupOtpStep2').style.display = 'block';
            } else {
                showMsg('signupOtpMsg', data.message || 'Failed to send code.', 'error');
            }
        } catch (e) {
            showMsg('signupOtpMsg', 'Network error. Is the backend running?', 'error');
        }
    };

    window.verifySignupOtp = async function() {
        const email = signupFormData.email;
        const otp   = document.getElementById('signupOtpCode').value.trim();
        if (!otp) { showMsg('signupOtpVerifyMsg', 'Enter the verification code.', 'error'); return; }
        showMsg('signupOtpVerifyMsg', 'Verifying...', 'info');
        try {
            const res  = await fetch(`${pythonURI}/api/otp/signup/verify`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ email, otp })
            });
            const data = await res.json();
            if (res.ok) {
                showMsg('signupOtpVerifyMsg', 'Email verified! Creating account...', 'success');
                document.getElementById('oauth-verification').classList.remove('show');
                document.getElementById('signupForm').style.display = '';
                window.signup();
            } else {
                showMsg('signupOtpVerifyMsg', data.message || 'Invalid code.', 'error');
            }
        } catch (e) {
            showMsg('signupOtpVerifyMsg', 'Network error.', 'error');
        }
    };

    window.resetSignupOtp = function() {
        document.getElementById('signupOtpStep2').style.display = 'none';
        document.getElementById('signupOtpStep1').style.display = 'block';
        document.getElementById('signupOtpCode').value = '';
        document.getElementById('signupOtpMsg').className = 'status-msg';
        document.getElementById('signupOtpMsg').textContent = '';
    };

    window.handleGoogleSignIn = function(response) {
        try {
            const b64  = response.credential.split('.')[1].replace(/-/g,'+').replace(/_/g,'/');
            const pad  = b64 + '='.repeat((4 - b64.length % 4) % 4);
            const info = JSON.parse(atob(pad));
            verifiedSchoolEmail = info.email;
            document.getElementById('oauth-status').innerHTML =
                `<span style="color:#86efac;">Verified: ${info.email}</span>`;
            setTimeout(() => {
                document.getElementById('oauth-verification').classList.remove('show');
                document.getElementById('signupForm').style.display = '';
                window.signup();
            }, 1200);
        } catch (e) {
            document.getElementById('oauth-status').innerHTML =
                '<span style="color:#fca5a5;">Error processing Google Sign-In. Try again.</span>';
        }
    };

    function updateBackendStatus(backend, status) {
        const el   = document.getElementById(`${backend}Status`);
        const icon = el.querySelector('.status-icon');
        const text = el.querySelector('.status-text');
        el.classList.remove('pending', 'success', 'error');
        const map = {
            pending: ['⏳', 'Flask'],
            success: ['✅', 'Flask ✓'],
            error:   ['❌', 'Flask ✗'],
        };
        el.classList.add(status);
        icon.textContent = map[status][0];
        text.textContent = map[status][1];
    }

    window.signup = function() {
        const btn = document.querySelector('#signupForm button[type="submit"]');
        if (btn) btn.disabled = true;
        updateBackendStatus('flask', 'pending');
        document.getElementById('overallStatus').classList.add('hidden');

        const data = Object.keys(signupFormData).length > 0 ? signupFormData : {
            name:              document.getElementById('name').value,
            uid:               document.getElementById('signupUid').value,
            sid:               document.getElementById('signupSid').value,
            school:            document.getElementById('signupSchool').value,
            email:             document.getElementById('signupEmail').value,
            password:          document.getElementById('signupPassword').value,
            kasm_server_needed: document.getElementById('kasmNeeded').checked,
        };

        fetch(`${pythonURI}/api/user`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(data)
        })
        .then(res => {
            if (res.ok) { updateBackendStatus('flask', 'success'); return res.json(); }
            return res.text().then(t => { throw new Error(t); });
        })
        .then(() => {
            const el = document.getElementById('overallStatus');
            el.className = 'overall-status success';
            el.textContent = 'Account created! You can now log in.';
            if (btn) btn.disabled = false;
        })
        .catch(err => {
            updateBackendStatus('flask', 'error');
            const el = document.getElementById('overallStatus');
            el.className = 'overall-status error';
            el.textContent = 'Account creation failed. ' + err.message;
            if (btn) btn.disabled = false;
        });
    };

    // ── Init ───────────────────────────────────────────────────────────────────

    window.addEventListener('load', function() {
        document.getElementById('signupPassword')
            ?.addEventListener('input', validatePasswordsDebounced);
        document.getElementById('confirmPassword')
            ?.addEventListener('input', validatePasswordsDebounced);

        if (window.google?.accounts) {
            window.google.accounts.id.initialize({
                client_id: GOOGLE_CLIENT_ID,
                callback: handleGoogleLogin
            });
        }
    });
</script>
