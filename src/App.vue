<template>
  <div class="ig">
   <img src="../src/jk.png" alt="Ghost Code Night">
  </div>
  <div class="container">
    <div class="container1">
      <div>
       <p class="p2">Biner 101010 <span class="load1">{{ TeksKetik }}</span></p>
      </div>
      <div class="in1">
       <button @click="KonversiBiner" :disabled="NoClick" class="bt1">Konversi</button>
       <input id="belom0" :disabled="NoInput" @keydown="CegahKetik" @input="HapusIsi" v-model="inputText" type="text" placeholder="Convert To Binary">
      </div>
      <textarea @click="HapusTeks" v-model="binaryText" name="" id="textsatu" readonly></textarea>
    </div>
    <div class="container2">
      <div>
       <p class="p1">Teks Huruf A-Z <span class="load2">{{ TeksKetik2 }}</span></p>
      </div>
      <div class="in2">
       <button @click="KonversiTeks" :disabled="NoClick2" class="bt2">Konversi</button>
       <input @paste="selain" :disabled="NoInput2" @input="HapusIsi2" v-model="inputText2" id="belom1" type="text" placeholder="Convert To Letters">
      </div>
      <textarea v-model="TeksText" @click="HapusTeks2" name="" id="textdua" readonly></textarea>
    </div>
  </div>
  <footer class="bawah">
   <div>
     <p class="copy">&copy; 2024 Ghost Code Night Hak Cipta</p>
   </div>
  </footer>
</template>
<script>
export default {
  data() {
    return {
      inputText: '',
      safeInput: '',
      binaryText: '',
      TeksKetik: '',
      clickCount: 0,
      NoInput: false,
      NoClick: false,
      inputText2: '',
      TeksText: '',
      clickCount2: 0,
      TeksKetik2: '',
      NoInput2: false,
      NoClick2: false,
    };
  },
  methods: {
    CegahKetik(event) {
      const CegahKarakter = ['/', '>', '<', '\\', ')', '(', '"', ']', ';', ':', '=', '*', '-', '.', ',', '&', '@', '#', '$', '%', '^', '+', '_', '~', '`'];
      if (CegahKarakter.includes(event.key)) {
        event.preventDefault();
      };
      const keyCode = event.keyCode || event.Which;
      if (keyCode >= 48 && keyCode <= 57) {
        event.preventDefault();
      }
    },
    HapusIsi() {
      this.inputText = this.inputText.replace(/[\/><\\)(]/g, '');
      this.safeInput = this.inputText.replace(/</g, '&lt;').replace(/>/g, '&gt;');
      if (this.inputText.length > 200) {
        alert("Your Text Is Too Much");
        this.inputText = '';
      }
    },
    KonversiBiner() {
      const isMathOperation = /[+\-*\/%^{}]/.test(this.inputText);
      if (isNaN(this.inputText) && !isMathOperation) {
        setTimeout(() => {
          let binaryResult = '';
          for (let i = 0; i < this.inputText.length; i++) {
            binaryResult += this.inputText[i].charCodeAt(0).toString(2) + ' ';
          }
          this.binaryText = binaryResult.trim();
        }, 6000);
      };
      if (this.inputText.trim() !== '') {
        this.NoClick = true;
        this.NoInput = true;
        let count = 5;
        let interval = setInterval(() => {
          this.TeksKetik = count;
          count--;
          if (count < 0) {
            clearInterval(interval);
            this.TeksKetik = '';
            this.NoClick = false;
            this.NoInput = false;
          };
        }, 1000);
      };
    },
    HapusTeks() {
      this.clickCount++;
      if (this.clickCount >= 2) {
        this.binaryText = '';
        this.clickCount = 0;
      }
    },
    selain(event) {
     const paste = (event.clipboardData || window.clipboardData).getData('text');
     const filter = paste.replace(/[^01]/g, '');
     this.inputText2 = filter;
     event.preventDefault();
     this.inputText2 = this.inputText2.replace(/<[^>]+>/g, '0'); 
    },
    KonversiTeks() {
      this.TeksText = this.inputText2.replace(/<[^>]+>/g, '0');
    },
    HapusTeks2() {
      this.clickCount2++;
      if (this.clickCount2 >= 2) {
        this.TeksText = '';
        this.clickCount2 = 0;
      }
    },
    KonversiTeks() {
      const binaryArray = this.inputText2.split(' '); 
      const textArray = binaryArray.map(binary => {
        if (/^[01]+$/.test(binary)) {
         const bits = binary.match(/.{1,8}/g);
         return bits.map(bit => String.fromCharCode(parseInt(bit, 2))).join('');
        } else {
         alert("auu");
         return null; 
        }
      });
      setTimeout(() => {
       this.TeksText = textArray.filter(text => text !== null).join('');
      }, 6000);
      if (this.inputText2.trim() != '') {
        const TesBiner = /^[01\s]+$/.test(this.inputText2.trim());
        if (TesBiner) {
          this.NoClick2 = true;
          this.NoInput2 = true;
          let count = 5;
          let interval = setInterval(() => {
           this.TeksKetik2 = count;
           count--;
            if (count < 0) {
             clearInterval(interval);
             this.TeksKetik2 = '';
             this.NoClick2 = false;
             this.NoInput2 = false;
            }
          }, 1000);

        } else {
          alert("nakal");
        }
      }
    },
    selain(event) {
      event.preventDefault();
      const pastedText = (event.clipboardData || window.clipboardData).getData('text');
      const filteredText = pastedText.replace(/[^0-9\s]/g, '');
      this.inputText2 = filteredText;
    },
    HapusIsi2() {
      if (this.inputText2.length > 200) {
        alert("Your Text Is Too Much");
        this.inputText2 = '';
      } else {
        this.inputText2 = this.inputText2.replace(/[^0-9\s]/g, '');
      }
    }, 
    disableRightClick(event) {
      event.preventDefault(); 
    }, 
  },
}
</script>
<style scoped>
.bawah {
  display: flex;
  justify-content: center;
}
.copy {
  animation: rainbow 2s infinite;
}
@keyframes rainbow {
  0% {
    color: white;
  }
  20% {
    color: black;
  }
  40% {
    color: antiquewhite;
  }
  60% {
    color: red;
  }
  80% {
    color: darkorchid;
  }
  100% {
    color: seagreen;
  }
}
.ig {
  display: flex;
  justify-content: center;
}
img {
  height: 300px;
  width: 300px;
  animation: images 2s infinite;
  pointer-events: none;
  overflow: hidden;
}
@keyframes images {
  0% {
    transform: rotateY(0deg);
  }
  100% {
    transform: rotateY(360deg);
  }
}
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
.container1 {
  flex-direction: column;
  padding-right: 10px;
}
.in1 {
  display: flex;
  padding-bottom: 10px;
  justify-content: center;
}
.container2 {
  flex-direction: column;
  padding-right: 10px;
}
.in2 {
  display: flex;
  padding-bottom: 10px;
  justify-content: center;
}
p {
  font-family: "Rubik Glitch Pop", system-ui;
  font-weight: 400;
  font-style: normal;
  font-size: 30px;
  text-align: center;
}
.in1, button {
  margin-right: 5px;
}
input {
  width: 400px;
  height: 20px;
  border-radius: 50px;
  font-weight: bold;
  text-align: center;
}
button {
  height: 27px;
  border-radius: 20px;
  color: white;
  font-weight: bold;
}
button:active {
  background-color: rgb(0, 255, 0);
}
.p1 {
  color: white;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.p1::selection {
  background-color: rgb(0, 255, 234);
}
.p2::selection {
  background-color: rgb(0, 255, 234);
}
.copy::selection {
  background-color: rgb(0, 255, 234);
}
.p2 {
  color: black;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.bt1 {
  background-color: black;
}
.bt2 {
  background-color: white;
  color: black;
}
textarea {
  border-radius: 10px;
  padding-left: 10px;
  padding-top: 10px;
  padding-right: 10px;
  resize: vertical;
  background-color: transparent;
  color: white;
  font-weight: bold;
  font-size: 20px;
}
#textsatu {
  color: black;
  width: 533px; 
  height: 312px;
}
#textdua {
  width: 533px; 
  height: 312px;
}
@media only screen and (max-width: 1209.33px) {
  .container {
    display: block;
  }
  .container1, .container2 {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  textarea {
    background-color: white;
    color: black;
  }
  p {
    animation: teks 1s infinite;
  }
  @keyframes teks {
  0% {
    color: white;
  }
  20% {
    color: black;
  }
  40% {
    color: antiquewhite;
  }
  60% {
    color: red;
  }
  80% {
    color: darkorchid;
  }
  100% {
    color: seagreen;
  }
}
}
@media only screen and (max-width: 600px) {
  #textsatu, #textdua {
    width: 400px;
  }
  input {
    width: 300px;
  }
  .copy {
    font-size: 20px;
  }
  .p2, .p1 {
    font-size: 20px;
  }
}
@media only screen and (max-width: 466px) {
  #textsatu, #textdua {
    width: 300px;
  }
  input {
    width: 200px;
  }
  .copy {
    font-size: 15px;
  }
}
@media only screen and (max-width: 352px) {
  #textsatu, #textdua {
    width: 250px;
  }
  input {
    width: 150px;
  }
  .copy {
    font-size: 12px;
  }
}
</style>
