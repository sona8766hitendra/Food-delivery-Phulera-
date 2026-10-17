<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Food Express - Welcome</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    }
    body {
      background: #f7f8fa;
      color: #333;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 16px;
    }
    .app-container {
      width: 100%;
      max-width: 420px;
      background: #ffffff;
      border-radius: 20px;
      padding: 24px;
      box-shadow: 0 8px 24px rgba(0,0,0,0.06);
    }
    .brand-header {
      text-align: center;
      margin-bottom: 24px;
    }
    .brand-logo {
      font-size: 42px;
    }
    .brand-title {
      font-size: 22px;
      font-weight: 700;
      color: #ff4757;
      margin-top: 6px;
    }
    .brand-sub {
      font-size: 13px;
      color: #747d8c;
    }
    .tab-switch {
      display: flex;
      background: #f1f2f6;
      border-radius: 12px;
      padding: 4px;
      margin-bottom: 20px;
    }
    .tab-btn {
      flex: 1;
      padding: 10px;
      border: none;
      background: transparent;
      font-size: 14px;
      font-weight: 600;
      color: #747d8c;
      border-radius: 10px;
      cursor: pointer;
    }
    .tab-btn.active {
      background: #ffffff;
      color: #ff4757;
      box-shadow: 0 2px 8px rgba(0,0,0,0.08);
    }
    .form-group {
      margin-bottom: 14px;
    }
    .form-group label {
      display: block;
      font-size: 12px;
      font-weight: 600;
      margin-bottom: 6px;
      color: #2f3542;
    }
    .form-group input {
      width: 100%;
      padding: 12px 14px;
      border: 1.5px solid #e4e7eb;
      border-radius: 10px;
      font-size: 14px;
      outline: none;
    }
    .form-group input:focus {
      border-color: #ff4757;
    }
    .referral-box {
      background: #fff0f3;
      padding: 12px;
      border-radius: 10px;
      border: 1px dashed #ff4757;
      margin-bottom: 16px;
    }
    .referral-box label {
      color: #d63031;
    }
    .btn-submit {
      width: 100%;
      background: #ff4757;
      color: white;
      border: none;
      padding: 13px;
      font-size: 15px;
      font-weight: 700;
      border-radius: 10px;
      cursor: pointer;
    }
    .msg-box {
      font-size: 13px;
      text-align: center;
      margin-top: 14px;
      padding: 10px;
      border-radius: 8px;
      display: none;
    }
    .msg-error {
      background: #ffeaa7;
      color: #d63031;
    }
    .msg-success {
      background: #e8f8f5;
      color: #2ed573;
    }
  </style>

  <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-auth-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
</head>
<body>

  <div class="app-container">
    <div class="brand-header">
      <div class="brand-logo">🍔</div>
      <div class="brand-title">Food Express</div>
      <div class="brand-sub">गरमा-गरम खाना, तुरंत आपके घर</div>
    </div>

    <div class="tab-switch">
      <button class="tab-btn active" id="tabSignup" onclick="switchTab('signup')">साइन अप</button>
      <button class="tab-btn" id="tabLogin" onclick="switchTab('login')">लॉगिन</button>
    </div>

    <div id="signupForm">
      <div class="form-group">
        <label>पूरा नाम</label>
        <input type="text" id="regName" placeholder="अपना नाम लिखें">
      </div>
      <div class="form-group">
        <label>ईमेल पता</label>
        <input type="email" id="regEmail" placeholder="name@example.com">
      </div>
      <div class="form-group">
        <label>पासवर्ड</label>
        <input type="password" id="regPassword" placeholder="कम से कम 6 अक्षर">
      </div>
      
      <div class="referral-box">
        <div class="form-group" style="margin-bottom:0;">
          <label>रेफ़रल कोड (वैकल्पिक)</label>
          <input type="text" id="regReferral" placeholder="अगर दोस्त का कोड है तो डालें" style="text-transform:uppercase;">
        </div>
      </div>

      <button class="btn-submit" onclick="handleSignUp()">अकाउंट बनाएँ</button>
    </div>

    <div id="loginForm" style="display: none;">
      <div class="form-group">
        <label>ईमेल पता</label>
        <input type="email" id="loginEmail" placeholder="name@example.com">
      </div>
      <div class="form-group">
        <label>पासवर्ड</label>
        <input type="password" id="loginPassword" placeholder="पासवर्ड दर्ज करें">
      </div>
      <button class="btn-submit" onclick="handleLogin()">लॉगिन करें</button>
    </div>

    <div id="alertBox" class="msg-box"></div>
  </div>

  <script>
    const firebaseConfig = {
      apiKey: "AIzaSyAM7E5qI7YuD8zRO93ns5EGe1y6kNFvVe8",
      authDomain: "food-delivery-c8836.firebaseapp.com",
      projectId: "food-delivery-c8836",
      storageBucket: "food-delivery-c8836.firebasestorage.app",
      messagingSenderId: "62361250063",
      appId: "1:62361250063:web:23a65f429cf5e42e5b767a",
      measurementId: "G-Q39K8T9QF9"
    };

    if (!firebase.apps.length) {
      firebase.initializeApp(firebaseConfig);
    }
    const auth = firebase.auth();
    const db = firebase.firestore();

    // लिंक से रेफरल कोड पढ़कर बॉक्स में अपने आप भरना (?ref=CODE)
    window.addEventListener('DOMContentLoaded', () => {
      const urlParams = new URLSearchParams(window.location.search);
      const refFromUrl = urlParams.get('ref');
      if (refFromUrl) {
        const refInput = document.getElementById('regReferral');
        if (refInput) {
          refInput.value = refFromUrl.toUpperCase();
        }
      }
    });

    function switchTab(tab) {
      const isSignup = tab === 'signup';
      document.getElementById('signupForm').style.display = isSignup ? 'block' : 'none';
      document.getElementById('loginForm').style.display = isSignup ? 'none' : 'block';
      document.getElementById('tabSignup').classList.toggle('active', isSignup);
      document.getElementById('tabLogin').classList.toggle('active', !isSignup);
      hideAlert();
    }

    function showAlert(msg, isSuccess = false) {
      const box = document.getElementById('alertBox');
      box.textContent = msg;
      box.className = `msg-box ${isSuccess ? 'msg-success' : 'msg-error'}`;
      box.style.display = 'block';
    }

    function hideAlert() {
      document.getElementById('alertBox').style.display = 'none';
    }

    function generateMyReferralCode(name) {
      const cleanName = (name.replace(/[^a-zA-Z]/g, '').slice(0, 3) || 'FD').toUpperCase();
      const randomNum = Math.floor(100 + Math.random() * 900);
      return cleanName + randomNum;
    }

    async function handleSignUp() {
      const name = document.getElementById('regName').value.trim();
      const email = document.getElementById('regEmail').value.trim();
      const password = document.getElementById('regPassword').value;
      const refCodeInput = document.getElementById('regReferral').value.trim().toUpperCase();

      if (!name || !email || !password) {
        showAlert("कृपया नाम, ईमेल और पासवर्ड भरें!");
        return;
      }
      if (password.length < 6) {
        showAlert("पासवर्ड कम से कम 6 अक्षरों का रखें!");
        return;
      }

      showAlert("अकाउंट बनाया जा रहा है...", true);

      try {
        const userCredential = await auth.createUserWithEmailAndPassword(email, password);
        const user = userCredential.user;
        const myCode = generateMyReferralCode(name);

        await db.collection("users").doc(user.uid).set({
          uid: user.uid,
          name: name,
          email: email,
          myReferralCode: myCode,
          referredBy: refCodeInput !== "" ? refCodeInput : null,
          referralCount: 0,
          hasFreeDelivery: false,
          walletBalance: 0,
          createdAt: firebase.firestore.FieldValue.serverTimestamp()
        });

        if (refCodeInput !== "") {
          const referrerSnapshot = await db.collection("users").where("myReferralCode", "==", refCodeInput).get();
          if (!referrerSnapshot.empty) {
            const referrerDoc = referrerSnapshot.docs[0];
            const referrerId = referrerDoc.id;
            const currentCount = referrerDoc.data().referralCount || 0;
            const newCount = currentCount + 1;

            let updateData = { referralCount: newCount };
            if (newCount >= 1) updateData.hasFreeDelivery = true;
            if (newCount === 3) updateData.walletBalance = firebase.firestore.FieldValue.increment(149);

            await db.collection("users").doc(referrerId).update(updateData);
          }
        }

        showAlert("अकाउंट बन गया! आगे बढ़ रहे हैं...", true);
        setTimeout(() => {
          window.location.href = "food.html";
        }, 1000);

      } catch (error) {
        showAlert("त्रुटि: " + error.message);
      }
    }

    async function handleLogin() {
      const email = document.getElementById('loginEmail').value.trim();
      const password = document.getElementById('loginPassword').value;

      if (!email || !password) {
        showAlert("ईमेल और पासवर्ड दोनों लिखें!");
        return;
      }

      try {
        await auth.signInWithEmailAndPassword(email, password);
        showAlert("लॉगिन सफल!", true);
        setTimeout(() => {
          window.location.href = "food.html";
        }, 1000);
      } catch (error) {
        showAlert("लॉगिन विफल: " + error.message);
      }
    }
  </script>
</body>
</html>
