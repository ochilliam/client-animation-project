<template>
    <div class="hero" id="container" :key="Math.random() * 100">
        <!--  ================ Scene One================= -->
        <div class="hero__wrapper" id="scene_one">
            <div class="hero__context-wrapper">
                <div class="hero__logo-wrapper">
                    <p class="hero__logo" id="logo">
                        {{ "valblv" }}
                    </p>
                    <p class="hero__logo--text">
                        {{ "scroll" }}
                        <svg
                            xmlns="http://www.w3.org/2000/svg"
                            xmlns:xlink="http://www.w3.org/1999/xlink"
                            aria-hidden="true"
                            focusable="false"
                            role="img"
                            width="1.5em"
                            height="2em"
                            preserveAspectRatio="xMidYMid meet"
                            viewBox="0 0 24 24"
                            style="transform: rotate(360deg);"
                        >
                            <path
                                d="M11 20V7.75L5.75 13L5 12.336l6.5-6.5l6.5 6.5l-.75.664L12 7.75V20h-1z"
                                fill="currentColor"
                            ></path>
                        </svg>
                    </p>
                </div>
                <!--  ==================Scene Two ================== -->
                <div class="hero__text-wrapper" id="scene_two">
                    <p class="hero__text" id="text">
                        {{ "how does it make you feel?" }}
                        <span>{{ "text" }}</span>
                    </p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    head: {
        script: [
            {
                src: "https://unpkg.com/gsap@3.5.0/dist/gsap.min.js",
                async: true
            },
            {
                src: "https://unpkg.com/gsap@3.5.0/dist/ScrollTrigger.min.js",
                async: true
            }
        ]
    },

    mounted() {
        this.$nextTick(() => {
            // Wait Untill GSAP Is Ready
            if (typeof gsap !== "undefined") {
                gsap.registerPlugin(ScrollTrigger);

                // Initialize Timeline Animation
                let Animation = gsap.timeline();

                // =============GSAP ANIMATION ==============//
                Animation.add(this.onScroll_logoScale)
                    .add(this.onScroll_textAppear)
                    .add(this.onScroll_textFade);
            }

            // Scroll Reset On Load | Only on Client
            if (process.client) {
                window.scroll({
                    top: 0,
                    left: 0,
                    behavior: "smooth"
                });
            }
        });
    },

    methods: {
        onScroll_logoScale() {
            return gsap
                .timeline()
                .to("#logo", {
                    scale: this.$el.clientWidth,
                    transformOrigin: "38.15% 45%", // differs on font
                    scrollTrigger: {
                        trigger: "#scene_one",
                        start: "top top",
                        end: "bottom top",
                        scrub: 1,
                        pin: true,
                        pinSpacing: false,
                        pinAnticipate: 1
                    }
                })
                .to(
                    "#container",
                    {
                        height: this.$el.clientHeight / 2.5 + "vh",
                        backgroundColor: "#4C51BF" // primary-color
                    },
                    "<1" // one second later
                );
        },

        onScroll_textAppear() {
            return gsap.fromTo(
                "#scene_two",
                { autoAlpha: 0, yPercent: -100 },
                {
                    autoAlpha: 1,
                    yPercent: -100,
                    scrollTrigger: {
                        trigger: "#scene_two",
                        start: "top 20%",
                        end: "top top",
                        toggleActions: "play complete play reverse", //onEnter, onLeave, onEnterBack, and onLeaveBack
                        markers: false
                    }
                }
            );
        },

        onScroll_textFade() {
            return gsap.to("#text", {
                yPercent: 100,
                autoAlpha: 0,
                scrollTrigger: {
                    trigger: "#scene_one",
                    start: "bottom 60%",
                    end: "100% 30%",
                    toggleActions: "play complete play reverse", //onEnter, onLeave, onEnterBack, and onLeaveBack
                    markers: false
                }
            });
        }
    }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Alata&display=swap");

/* Fix for IOS View Height | https://css-tricks.com/css-fix-for-100vh-in-mobile-webkit/
 */
/* body {
    min-height: 100vh !important;
    overflow-x: hidden;
    font-family: "Alata", sans-serif;
    min-height: -webkit-fill-available !important;
}
html {
    height: -webkit-fill-available !important;
} */

/*  page parent for CSS custom propertyies  */
.hero {
    font-size: 16px;
    font-family: inherit;
    height: 120vh;
    width: 100%;

    --bg-white: #fff;
    --bg-primary: #4c51bf;

    --color-primary: #4c51bf;
    --color-white: #fff;

    --font-sm: 0.75rem;
    --font-lg: 1.125rem;
    --font-xl: 1.25rem;
    --font-2xl: 1.375rem;
    --font-3xl: 1.875rem;

    --tracking-wider: 0.3ch;
    --tracking-widest: 0.8ch;

    --leading-loose: 2;
}

/* ============= Wrappers ============== */

.hero__wrapper {
    background-color: var(--bg-white);
    width: 100%;
    height: 100%;
}

.hero__context-wrapper {
    height: 100%;
    margin: 0 auto;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

/* ============ Hero Logo ============== */

.hero__logo-wrapper {
    width: auto;
    height: auto;
    text-align: center;
    text-transform: uppercase;
    color: var(--color-primary);
    padding-left: 0.8rem;
}

.hero__logo {
    display: block;
    font-size: var(--font-3xl);
    color: inherit;
    font-weight: bold;
    padding-bottom: 0.5rem;
    line-height: var(--leading-loose);
    letter-spacing: var(--tracking-widest);
}

.hero__logo--text {
    display: flex;
    justify-content: center;
    align-items: center;
    color: inherit;
    letter-spacing: var(--tracking-wider);
    font-size: var(--font-sm);
}

.hero__logo--text svg {
    display: inline;
    padding-bottom: 0.25rem;
}

/* ================ Hero Text ==============  */

.hero__text-wrapper {
    color: var(--color-white);
    text-align: center;
    text-transform: uppercase;
}

.hero__text {
    display: block;
    padding-bottom: 0.5rem;
    line-height: var(--leading-loose);
    letter-spacing: var(--tracking-wider);
}

.hero__text span {
    display: block;
    font-size: var(--font-sm);
}

/* ========= Utility ========== */
.hidden {
    display: none;
}
</style>
