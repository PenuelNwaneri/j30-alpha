#JavaScript 30
## By Wes Bos
Started 9-28-2017

## Getting Setup
1) Download starter files 
2) Download modern browser

- Take course at any pace you want
- People learn in different ways: 
1) watch complete lesson and build
2) Built with guidelines
3) Watch and build simultaneously
- There are many ways to complete the challenges
- We are avoiding design patterns, classes, closures, frameworks
- Don't use jQuery, lodash, etc.
- This course can use plain JavaScript
- Find an accountabilabuddy

## 01 - JavaScript Drum Kit
- Building a drum kit to play sounds on keypress
- Keycode.info shows the key you pressed on keyboard

**Data Attribute**
- This is not a standard element and you won't find on MDN
- We are using data attribute to link to audio element

**Listen for keydown event**
```
<script>
    window.addEventListener('keydown', function (e) {
        console.log(e);
        console.log(e.keyCode);
    })
</script>
```

**Using attribute selector**
```
<script>
    window.addEventListener('keydown', function (e) {
        const audio = document.querySelector('audio[data-key="${e.keyCode}"]');
        console.log(audio);
    })
</script>
```

**Play audio or stop if no data key code present**
```
<script>
    window.addEventListener('keydown', function (e) {
        const audio = document.querySelector('audio[data-key="${e.keyCode}"]');
        if(!audio) return; // stop the function from running all together

        audio.play();
    })
</script>
```

**Be able to hit the keys to play multiple time and not wait to finish**
```
<script>
    window.addEventListener('keydown', function (e) {
        const audio = document.querySelector('audio[data-key="${e.keyCode}"]');
        if(!audio) return; // stop the function from running all together
        audio.currentTime = 0; // rewind to the start
        audio.play();
    })
</script>
```

**Toggle classes to add transitions when pushing a key**
- You can use key.classList.add(), key.classList.remove(), or key.classList.toggle()
```
<script>
  window.addEventListener('keydown', function(e) {
    const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
    if(!audio) return; // stop the function from running all together
    audio.currentTime = 0; // rewind to the start
    audio.play();
    key.classlist.add('playing');
  })
</script>
```

**Add listener for when there is a transition and stop it**
- You can't add an event listener on an array of elements (Node List as he refers it)
- You just can't listen to all them
- So you have to loop through every single element in the array and attach an event listener on each
- Then we will listen for a transition end and then run removeTransition()
```
<script>
  window.addEventListener('keydown', function(e) {
    const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
    if(!audio) return; // stop the function from running all together
    audio.currentTime = 0; // rewind to the start
    audio.play();
    key.classlist.add('playing');

    const keys = querySelectorAll('.key');
    keys.forEach(key => key.addEventListener('transitionend', removeTransition));
  })
</script>
```

**Now we add a removeTransition function**
```
<script>
  window.addEventListener('keydown', function(e) {
    const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
    if (!audio) return;
    audio.currentTime = 0;
    audio.play();
    key.classlist.add('playing');

    function removeTransition(e) {
      if (e.propertyName !== 'transform') return;
      this.classList.remove('playing');
    }

    const keys = querySelectorAll('.key');
    keys.forEach(key => key.addEventListener('transitionend', removeTransition));
  })
</script>
```

**Code refactoring**
```
<script>  
function playSound(e) {
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
  if (!audio) return;
  audio.currentTime = 0;
  audio.play();
  key.classList.add('playing');
}

function removeTransition(e) {
  if (e.propertyName !== 'transform') return;
  this.classList.remove('playing');
}

const keys = document.querySelectorAll('.key');
keys.forEach(key => key.addEventListener('transitionend', removeTransition));
window.addEventListener('keydown', playSound);
</script>
```