<template>
    <div class="username-container">
        <label>Enter your username</label>
        <input 
            type="text" 
            v-model="username" 
            placeholder="Enter username" 
        />
    </div>
    <div class="password-strength">
        <label>Enter your password</label>
        <input 
            type="text" 
            v-model="password" 
            placeholder="Enter password" 
        />
        <button @click="checkPasswordStrength">
            Check Strength
        </button>
    </div>
    <div class="error-container">
        <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
    </div>

    <div v-if="passwordScoreMessage" class="password-score">
        <p>
        Password Strength: <span :style="{ color: passwordScoreColor }">{{ passwordScoreMessage }}</span>
      </p>
      <p>
        Score: <span :style="{ color: passwordScoreColor }">{{ passwordScore }} / 10</span>
      </p>
    </div>
  </template>
  
  <script>
  export default {
    name: 'PasswordStrength',
    data() {
      return {
        username: '',
        password: '',
        passwordScore: 0,
        passwordScoreMessage: '',
        passwordScoreColor: '',
        commonWords: ["password", "12345", "123456", "qwerty", "admin", "welcome", "emitknowledge", "abc123", "abc"],
        errorMessage: ''
      }
    },
    methods: {
        calculatePasswordScore(score) {
        switch(score) {
            case 0:
                this.passwordScoreMessage = 'Password is too weak.';
                this.passwordScoreColor = '#FCED58';
                break;
            case 1:
                this.passwordScoreMessage = 'Password is weak.';
                this.passwordScoreColor = '#FCED58';
                break;
            case 3:
                this.passwordScoreMessage = 'Password is not too strong.';
                this.passwordScoreColor = '#F9A825';
                break;
            case 6:
                this.passwordScoreMessage = 'Password is strong.';
                this.passwordScoreColor = '#00BCD4';
                break;
            case 10:
                this.passwordScoreMessage = 'Password is very strong.';
                this.passwordScoreColor = '#439E47';
                break;
        }
      },
      checkPasswordStrength() {
        let passwordLength = this.password.length;
        const passwordRegEx = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*]).{8,}$/;
        const lowerPassword = this.password.toLowerCase();
        const lowerUsername = this.username.toLowerCase();
        this.passwordScore = 0;

        if (passwordLength == 0) {
            this.errorMessage = 'Please enter your password.';
            return;
        }

        if (this.username && lowerPassword === lowerUsername) {
            this.errorMessage = 'Password should not be identical to the username.';
            this.calculatePasswordScore(this.passwordScore);
            return;
        }

        this.passwordScore += 1;

        if (passwordLength < 8) {
            this.errorMessage = 'Password must contain at least 8 characters.';
            this.calculatePasswordScore(this.passwordScore);
            return;
        }

        this.passwordScore += 2;

        if (!passwordRegEx.test(this.password)) {
            this.errorMessage = 'Password needs to contain at least 1 lowercase letter, 1 uppercase letter, 1 digit and 1 special character.';
            this.calculatePasswordScore(this.passwordScore);
            return;
        }

        this.passwordScore += 3;

        const containsCommonWord = this.commonWords.some(word => lowerPassword.includes(word));

        if (containsCommonWord) {
            this.errorMessage = 'Please avoid using common words or patterns.';
            this.calculatePasswordScore(this.passwordScore);
            return;
        }

        this.passwordScore += 4;

        this.errorMessage = '';

        this.calculatePasswordScore(this.passwordScore);
      }
    }
  }
  </script>
  
  <style scoped>
  .username-container {
    margin-bottom: 15px;
    display: flex;
    gap: 10px;
  }
  .password-strength {
    display: flex;
    gap: 10px;
  }
  label {
    margin-top: 5px;
  }
  input {
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  button {
    padding: 8px 12px;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  button:hover {
    background-color: #45a049;
  }
  .error-container {
    margin-top: 5px;
  }
  .error-message {
    color: red;
  }
  </style>
  