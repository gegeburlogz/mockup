<script setup>
import { ref, onMounted } from "vue";

const count = ref(0);
let intervalId;
const removeAnim = ref([])
const start = ref(false), toScale = ref(false), isScrolling = ref(false);

onMounted(() => {
    intervalId = setInterval(() => {
        count.value++;
        if (count.value === 5) {
            removeAnim.value.forEach((el, index) => {
                el.classList.remove("anim1", "anim2", "anim3");
            });
            start.value = true
            const headerName = document.querySelector('.headerName');
            let rev = setTimeout(() => {
                headerName.style.display = 'none';
                clearTimeout(rev)
            }, 2000);
            clearInterval(intervalId);
        }
    }, 1000);
});


const handleScroll = (e) => {
    if (!start.value) {
        e.preventDefault();
    } else {
        if (e.type === "scroll") {
            const totalWindowH = window.innerHeight / 2;
            const scrollY = window.scrollY;
            const imageContainer = document.getElementById('type_two_cont');
            const containerTop = imageContainer.offsetTop;
           
            if (scrollY < totalWindowH) {
                document.getElementById("type_one").style.transform = `scale(${Math.min((scrollY / totalWindowH) * 15, 15)})`;
                document.getElementById("type_two").style.transform = `scale(0)`
                document.getElementById("type_one").style.opacity = Math.min((scrollY / totalWindowH) * 12, 15)
                document.getElementById("type_two").style.opacity = 0


            }
            if (scrollY >= totalWindowH) {
                document.getElementById("type_one").style.opacity = 0
                let type_two_scale = Math.min(((scrollY - totalWindowH) / totalWindowH) * 12, 15)
                document.getElementById("type_two").style.transform = `scale(${type_two_scale})`;
                document.getElementById("type_two").style.opacity = type_two_scale > 14 ? 0 : type_two_scale
            }
            if (scrollY > 998) {
                const scrollInSection = scrollY - containerTop;
                document.getElementById("flyingBg").style.bottom = `${ -1020 + (scrollInSection / 5)}vh`;
            }
        }
    }
};
onMounted(() => {
    window.addEventListener("scroll", handleScroll, { passive: false });
    window.addEventListener("wheel", handleScroll, { passive: false });
    window.addEventListener("touchmove", handleScroll, { passive: false });
    window.scrollTo({ top: 0, behavior: "smooth" });
});

onUnmounted(() => {
    window.removeEventListener("scroll", handleScroll);
    window.removeEventListener("wheel", handleScroll);
    window.removeEventListener("touchmove", handleScroll);
});
</script>
<template>
    <div class="headerPoint">
        <div :class="{ 'headerName': true, 'toScale': start }">
            <div class="headerTxt flex justify-center items-center h-screen silkscreen-regular text-white ">
                <span>L</span>
                <span>i</span>
                <span>n</span>
                <span>k</span>
                <span :class="{ 'hidden': start, 'show': !start }">i</span>
                <span :class="{ 'hidden': start, 'show': !start }">n</span>
                <span :class="{ 'hidden': start, 'show': !start }">g</span>
                <span v-for="(dot, index) in 3" :key="index" ref="removeAnim"
                    :class="`anim${index + 1} ${start ? 'hidden' : ''}`">.</span>
                <span :class="{ 'hidden': !start, 'show': start }">S</span>
                <span :class="{ 'hidden': !start, 'show': start }">t</span>
                <span :class="{ 'hidden': !start, 'show': start }">a</span>
                <span :class="{ 'hidden': !start, 'show': start }">r</span>
                <span :class="{ 'hidden': !start, 'show': start }">t</span>
            </div>
        </div>
        <div class="main-cont">
            <video autoplay muted loop>
                <source src="~assets/images/bg_video.mp4" type="video/mp4">
            </video>
            <div id="type_one" class="ahi flex justify-center items-center h-screen text-white silkscreen-regular">
                One
            </div>
            <div id="type_two" class="ahi flex justify-center items-center h-screen text-white silkscreen-regular">
                Two
            </div>
            <div class="ahi_cont"></div>
            <div class="ahi_cont"></div>
        </div>
        <div id="type_two_cont" class="two_cont josefin-sans-regular">
            <p>In the heart of a quiet town, nestled between a bakery and a bookstore, stood an old clock shop. Its
                owner, Mr. Alistair Finch, was a peculiar man—soft-spoken, meticulous, and always smelling faintly of
                oil and cedarwood.</p>

            <p>For years, townsfolk visited his shop to repair their watches and antique clocks, marveling at how time
                seemed to run smoother after his touch. But no one knew his secret.</p>

            <p>One evening, a curious boy named Oliver peeked through the window after hours and gasped. Mr. Finch
                wasn’t just fixing clocks—he was <strong>whispering to them</strong>. And they were whispering back.</p>

            <p>Startled, Oliver pushed the door open, and the chimes rang softly. Mr. Finch turned, smiling. “Ah, you’ve
                found out.”</p>

            <p>Oliver hesitated. “What… what are you doing?”</p>

            <p>The old man chuckled, placing a delicate golden pocket watch on the counter. “Time is more than numbers
                on a dial. It listens. It remembers.” He handed the watch to Oliver. “Hold it close, and you’ll hear its
                story.”</p>

            <p>With wide eyes, Oliver lifted the watch to his ear—and suddenly, he wasn’t in the shop anymore. He was
                <strong>standing in a different time</strong>, watching a young Mr. Finch receive the same watch from
                his grandfather, who whispered:
            </p>

            <p><em>“Time isn’t just measured. It’s lived.”</em></p>

            <p>With a gasp, Oliver found himself back in the present, holding the warm watch in trembling fingers.</p>

            <p>Mr. Finch winked. “Now you know. Every clock has a story. Would you like to learn how to listen?”</p>

            <p>And from that night on, Oliver did.</p>
            <img id="flyingBg" src="~assets/images/bg1.png" />
        </div>
    </div>
</template>
<style scoped>
.headerPoint {
    position: relative;
}

.hidden {
    visibility: hidden;
}

.show {
    visibility: visible;
}

.headerPoint .headerName {
    position: absolute;
    z-index: 3;
    background-color: #227B94;
    min-height: 100vh;
    width: 100vw;
    transition: transform 2s cubic-bezier(0.25, 0.1, 0.25, 1), opacity 2s cubic-bezier(0.25, 0.1, 0.25, 1);
}

.toScale {
    transform: scale(15);
    opacity: 0;
}

.headerPoint .headerName .headerTxt {
    font-size: 8em;
}

.headerPoint .headerName .headerTxt .anim1 {
    animation: blink-animation 1s steps(2, start) infinite;
    -webkit-animation: blink-animation 1s steps(2, start) infinite;
}

.headerPoint .headerName .headerTxt .anim2 {
    animation: blink-animation 1s steps(3, start) infinite;
    -webkit-animation: blink-animation 1s steps(3, start) infinite;
}

.headerPoint .headerName .headerTxt .anim3 {
    animation: blink-animation 1s steps(5, start) infinite;
    -webkit-animation: blink-animation 1s steps(5, start) infinite;
}

.main-cont {
    z-index: 1;
    position: relative;
    min-height: 100vh;
    width: 100vw;
    overflow-y: scroll;
    scroll-snap-type: y mandatory;
    scrollbar-width: none;
}

.main-contr::-webkit-scrollbar {
    display: none;
}

.main-cont video {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    max-width: none;
    min-width: 100%;
    min-height: 100%;
    z-index: 1;
}

.main-cont .ahi {
    position: fixed;
    top: 0;
    left: 0;
    height: 100vh;
    width: 100vw;
    z-index: 2;
    font-size: 9em;
    text-shadow: 2px 2px 9px rgb(10, 47, 58);
    opacity: 1;
    transform: scale(0);
    transition: opacity 0.5s ease-out;

}

.main-cont .ahi_cont {
    position: relative;
    height: 100vh;
    width: 100vw;
    z-index: 2;
    scroll-snap-align: start;
    font-size: 9em;
    text-shadow: 2px 2px 9px rgb(10, 47, 58);
}

.two_cont {
    position: relative;
    min-height: 100vh;
    width: 100vw;
    font-size: 3em;
    z-index: 3;
    padding: 10% 20%;
    background-color: #f4f4f4;
    box-shadow: 0px -20px 15px rgba(244, 244, 244, 1);
    overflow: hidden;
}

.two_cont img {
    z-index: 1;
    position: absolute;
    width: 100%;
    bottom: -120vh;
    left: 0;
    transition: bottom 0.1s ease-in-out;
}

.two_cont p {
    position: relative;
    z-index: 3;
    color:white;
    mix-blend-mode: difference;
    -webkit-font-smoothing:antialiased;
    -moz-osx-font-smoothing:grayscale;
}

@keyframes blink-animation {
    to {
        visibility: hidden;
    }
}

@-webkit-keyframes blink-animation {
    to {
        visibility: hidden;
    }
}
</style>