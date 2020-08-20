<template>
    <div class="hero" ref="container" id="container">
        <!--  ================ Scene One================= -->
        <div class="hero__wrapper" ref="panel" id="scene_one">
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
    // ============== In case you dont have GSAP ====================//
    // head: {
    //     script: [
    //         {
    //             src: "https://unpkg.com/gsap@3.5.0/dist/gsap.min.js",
    //             async: true
    //         },
    //         {
    //             src: "https://unpkg.com/gsap@3.5.0/dist/ScrollTrigger.min.js",
    //             async: true
    //         }
    //     ]
    // },

    mounted() {
        this.$nextTick(() => {
            gsap.registerPlugin(ScrollTrigger);

            const { style: contanerStyle } = this.$refs.container;
            const { style: panelStyle } = this.$refs.panel;

            panelStyle.setProperty("height", window.innerHeight + "px");
            document.body.style.setProperty(
                "height",
                window.innerHeight * 2 + "px"
            );

            window.onresize = () =>
                panelStyle.setProperty("height", window.innerHeight + "px");

            requestAnimationFrame(() => {
                contanerStyle.setProperty(
                    "bottom",
                    document.documentElement.scrollTop * -1 + "px"
                );
            });

            // Initialize Timeline Animation
            let Animation = gsap.timeline();

            // =============GSAP ANIMATION ==============//
            Animation.add(this.onScroll_logoScale)
                .add(this.onScroll_textAppear)
                .add(this.onScroll_textFade);
        });
    },

    methods: {
        onScroll_logoScale() {
            return gsap
                .timeline()
                .to("#logo", {
                    scale: document.documentElement.clientWidth,
                    transformOrigin: "38.15% 45%",
                    scrollTrigger: {
                        trigger: "#scene_one",
                        start: "12% center",
                        end: "bottom 0%",
                        scrub: 1,
                        pin: true,
                        onLeave: () => gsap.to("#logo", { autoAlpha: 0 }),
                        onEnterBack: () => gsap.to("#logo", { autoAlpha: 1 }),
                        toggleActions: "play complete reverse complete" //onEnter, onLeave, onEnterBack, and onLeaveBack
                    }
                })
                .to("#container", {
                    height: window.innerHeight * 2.5 + "px",
                    scrollTrigger: {
                        trigger: "#scene_one",
                        start: "10% center",
                        end: "bottom 0%",
                        scrub: true
                    }
                });
        },

        onScroll_textAppear() {
            return gsap.fromTo(
                "#scene_two",
                { autoAlpha: 0, yPercent: -130 },
                {
                    autoAlpha: 1,
                    yPercent: -130,
                    scrollTrigger: {
                        trigger: "#scene_two",
                        start: "center 20%",
                        end: "bottom 0%",
                        scrub: true,
                        toggleActions: "play reverse play reverse" //onEnter, onLeave, onEnterBack, and onLeaveBack
                    }
                }
            );
        },

        onScroll_textFade() {
            return gsap.to("#text", {
                yPercent: 50,
                autoAlpha: 0,
                scrollTrigger: {
                    trigger: "#scene_one",
                    start: "center top",
                    end: "bottom 0%",
                    scrub: true,
                    toggleActions: "play reverse play reverse" //onEnter, onLeave, onEnterBack, and onLeaveBack
                }
            });
        }
    }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Alata&display=swap");

/* ================= page parent for CSS custom propertyies ================ */
.hero {
    font-size: 16px;
    font-family: "Alata", sans-serif;
    position: fixed;
    bottom: 0;
    left: 0;
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
    position: relative;
    display: block;
    margin: 0 auto;
    text-align: center;
    top: 40%;
}

.hero__context-wrapper {
    width: 100%;
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
    letter-spacing: var(--tracking-wider);
    line-height: var(--leading-loose);
}

.hero__text span {
    display: block;
    font-size: var(--font-sm);
}
</style>
