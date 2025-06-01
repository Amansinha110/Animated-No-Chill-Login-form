<h1>Animated No-Chill Login Form</h1>

<P>My name is Aman Kumar, and I have developed an interactive and highly animated login form using JavaScript, designed with AI-based enhancements for an engaging user experience. This "No-Chill" Login Form is not your ordinary login screen—it’s filled with dynamic animations, unpredictable behaviors, and AI-driven interactions to challenge the user and make logging in a fun, unconventional experience.</P>

<h2>Concept and Features</h2>

<P>Most login forms are predictable and static. The Animated No-Chill Login Form breaks the norm by introducing animated buttons, intelligent prompts, and chaotic transitions to keep users entertained. This unique interface includes the following features:

Unpredictable Button Movements: The login button shifts positions when hovered over, making the user "chase" it.

AI-Based Reaction System: AI detects user hesitation or retries and responds accordingly with encouraging or teasing messages.

Shaking Inputs: The form shakes aggressively if incorrect credentials are entered, making the login attempt feel interactive.

Dynamic Background Changes: Colors and effects shift based on user behavior, creating a lively atmosphere.

Smart Captcha Alternative: Instead of traditional CAPTCHA, AI analyzes typing speed and patterns to detect bots.

Error Animations: If the user enters incorrect details too many times, animations become more exaggerated, reacting to frustration levels.</P>

<h3>Implementation Overview</h3>

<P>HTML: Provides the basic form structure.

CSS: Implements crazy animations and dynamic effects.

JavaScript: Drives the AI logic, unpredictable interactions, and responsive animations.</P>

<img src = "https://github.com/Amansinha110/Animated-No-Chill-Login-form/blob/master/Screenshot%202025-06-01%20063714.png">
<a href = "https://amansinha110.github.io/Animated-No-Chill-Login-form/"><img src = "https://github.com/Amansinha110/Animated-No-Chill-Login-form/blob/master/OIP.jpeg"></a>
<h1>Click Blue Button To Run </h1>

```javascript
const containerEl = document.querySelector('.container');
const checkboxEl = document.querySelector('.form-container .form-row input[type="checkbox"]');
const nameEl = document.querySelector('.form-container .form-row input[name="name"]');
const emailEl = document.querySelector('.form-container .form-row input[name="email"]');
const submitBtn = document.querySelector('.form-container .form-row input[type="submit"]');

const sprayer = document.querySelector('.sprayer');
const sprayHandContainer = document.querySelector('.spray-hand-container');
const sprayLines = Array.from(document.querySelectorAll('.spray-line'));
const sprayBubbles = Array.from(document.querySelectorAll('.spray-bubble'));

const pushingHand = document.querySelector('.pushing-hand');
const sprayerHead = document.querySelector('.sprayer-head');
const gearsContainer = document.querySelector('svg .gears');
const gearConnector = document.querySelector('.gear-connector');

const pullSystemContainer = document.querySelector('.pull-system');

const checkboxPullLine = document.querySelector('.checkbox-pull-line');
const checkboxPullCircle = document.querySelector('.checkbox-pull-circle');
const btnPullLine = document.querySelector('.submit-btn-connector');
const btnHandlerCircle = document.querySelector('.submit-btn-circle');

const spiralContainer = document.querySelector('.spiral-container');
const weightBigContainer = document.querySelector('.weight-big-container');

const scalesContainer = document.querySelector('.scales-container');
const scalesLine = document.querySelector('.scales-moving-line');
const weightBig = document.querySelector('.weight-big');
const spiralPath = document.querySelector('.spiral-path');

const carContainer = document.querySelector('.car-container');
const car = document.querySelector('.car');
const carInclineWrapper = document.querySelector('.car-container g');
const timingChains = Array.from(document.querySelectorAll('.timing-chain'));
const reelsConnector = document.querySelector('.reels-connector');
const carWeightConnector = document.querySelector('.car-weight-connector');

const grabbingHand = document.querySelectorAll('.grabbing-hand');
const grabbingHandOpenFingers = Array.from(document.querySelectorAll('.grabbing-hand-finger-open'));
const grabbingHandClosedFingers = Array.from(document.querySelectorAll('.grabbing-hand-finger-closed'));


layoutPreparation();
scaleToFit();
window.onresize = scaleToFit;

function scaleToFit() {
    const h = 800;

    if (window.innerHeight < h) {
        gsap.set(containerEl, {
            scale: window.innerHeight / h,
            transformOrigin: "50% 75%"
        })
    }

}


let sprayRepeatCounter = 0;
const state = {
    handClosed: false,
    sumbitBtnOnPlace: false,
    sumbitBtnTextOpacity: 0,
    pullProgress: 0
}
let nameValid = false;
let emailValid = false;

const emailTl = createEmailTl();
const gearsTls = createGearsTimelines();
createPullingTimeline(state.handClosed, checkboxEl.checked);


checkboxEl.addEventListener('change', () => {
    createPullingTimeline(state.handClosed, checkboxEl.checked);
})

nameEl.addEventListener('input', () => {
    nameValid = nameEl.value.length > 3;
    if (nameValid) {
        nameEl.classList.add("valid");
        gearsTls.forEach(tl => {
            if (tl.paused()) {
                tl.play();
                gsap.fromTo(tl, {
                    timeScale: 0
                }, {
                    timeScale: 1
                })
            }
        })
    } else {
        nameEl.classList.remove("valid");
        gearsTls.forEach(tl => {
            if (!tl.paused()) {
                gsap.to(tl, {
                    timeScale: 0,
                    onComplete: () => {
                        tl.pause();
                    }
                })
            }
        })
        sprayRepeatCounter = 0;
        gsap.to(submitBtn, {
            duration: .3,
            color: "rgba(0, 0, 0, " + 0 + ")"
        })
    }
})

emailEl.addEventListener('input', () => {
    const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
    emailValid = emailRegex.test(emailEl.value);
    if (emailValid) {
        emailTl.play();
        emailEl.classList.add("valid");
    } else {
        emailTl.reverse();
        emailEl.classList.remove("valid");
    }
})

submitBtn.addEventListener('click', () => {
    if (emailValid && checkboxEl.checked && nameValid && sprayRepeatCounter > 1) {
        gsap.to("svg > *", {
            duration: .1,
            opacity: 0,
            stagger: {
                each: 0.03,
                from: 'random',
                ease: 'none',
            }
        })
        gsap.to(".form-row", {
            delay: .4,
            duration: .1,
            opacity: 0,
            stagger: .1
        })
    }
})


function layoutPreparation() {
    gsap.set(pullSystemContainer, {
        x: 375,
        y: 646
    })
    gsap.set(sprayHandContainer, {
        x: 700,
        y: 621
    })
    gsap.set(sprayer, {
        x: -59.5,
        y: 53
    })
    gsap.set(carContainer, {
        x: 190,
        y: 802,
    })
    gsap.set(scalesContainer, {
        x: 170,
        y: 710,
    })
    gsap.set(grabbingHand, {
        x: 297,
        y: 830
    })
    gsap.set(grabbingHandClosedFingers, {
        opacity: 0
    })
    gsap.set(spiralContainer, {
        x: 305,
        y: 435,
        svgOrigin: "14 14",
        scaleX: -1,
    })
    gsap.set(weightBigContainer, {
        x: 305,
        y: 435,
    })
    gsap.set(submitBtn, {
        color: "rgba(0, 0, 0, " + 0 + ")"
    })
    gsap.set([sprayLines, sprayBubbles], {
        opacity: 0
    })
    gsap.set(timingChains[0], {
        attr: {
            "stroke-width": "5",
            "stroke-dasharray": "0 12",
        }
    })
    gsap.set(timingChains[1], {
        attr: {
            "stroke-width": "5",
            "stroke-dasharray": "0 12",
        }
    })
    gsap.set(checkboxPullLine, {
        attr: {
            y1: -105,
            y2: 44
        }
    });
    gsap.set(submitBtn, {
        transformOrigin: "100% 0%",
        rotation: -90
    })
    gsap.set(checkboxPullCircle, {
        y: 44
    });
}
```
<h1>AI Enhancements</h1>

<P>To enhance AI-driven interactions, you could integrate:

Sentiment Analysis: Detect user frustration and adjust animations dynamically.

Behavior Tracking: Change difficulty based on typing speed and login attempts.

Neural Network Predictions: Generate random AI prompts based on user interactions.</P>
