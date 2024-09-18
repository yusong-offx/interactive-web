<script lang="ts">
    interface CardSize {
        width: number;
        height: number;
    }

    let objectCard: HTMLDivElement;
    let objectCardSize: CardSize = { height: 480, width: 270 };

    let mouseX: number = 0;
    let mouseY: number = 0;

    let rect: DOMRect;

    const cardMouseMoveEventHandler = (event: MouseEvent) => {
        rect = objectCard.getBoundingClientRect();
        mouseX = event.clientX - rect.left - objectCardSize.width / 2;
        mouseY = event.clientY - rect.top - objectCardSize.height / 2;

        const rotateX = (-mouseY / objectCardSize.height) * 50;
        const rotateY = (mouseX / objectCardSize.width) * 50;

        const updateTransform = () => {
            objectCard.style.transform = `perspective(10000px) rotateX(${rotateX}deg) rotateY(${rotateY}deg)`;
            const dx = (mouseX / objectCardSize.width) * 100;
            const dy = (mouseY / objectCardSize.height) * 100;

            objectCard.style.boxShadow = `
                ${-dx}px ${-dy}px 20px rgba(0, 0, 0, 0.3),
                ${dx}px ${dy}px 30px rgba(0, 0, 0, 0.2)`;

            const angle = Math.atan2(dy, dx) * (180 / Math.PI) + 180;
            objectCard.style.background = `linear-gradient(${angle}deg, #6b73ff, #000dff)`;
        };

        requestAnimationFrame(updateTransform);
    };

    const cardMouseLeaveEventHandler = (event: MouseEvent) => {
        objectCard.style.transform =
            "perspective(10000px) rotateX(0deg) rotateY(0deg)";
        objectCard.style.boxShadow = "0 10px 15px rgba(0, 0, 0, 0.1)";
        objectCard.style.background =
            "linear-gradient(135deg, #6b73ff, #000dff)";
    };
</script>

<div
    class="w-screen h-screen bg-yellow-100 flex flex-col justify-center items-center"
>
    <div class="-translate-y-[120px] flex flex-col items-center">
        <h1>x : {mouseX} y : {mouseY}</h1>
        <h1>
            {rect?.left.toFixed(2)}
            {rect?.top.toFixed(2)}
            {rect?.right.toFixed(2)}
            {rect?.bottom.toFixed(2)}
        </h1>
    </div>
    <div
        id="cardContainer"
        class="flex justify-center items-center bg-red-400"
        on:mouseleave={cardMouseLeaveEventHandler}
        role="region"
    >
        <div
            bind:this={objectCard}
            id="card"
            class="flex justify-center h-[{objectCardSize.height}px] w-[{objectCardSize.width}px] text-[2rem] text-white items-center"
            role="region"
            on:mousemove={cardMouseMoveEventHandler}
        >
            Hello World!
        </div>
    </div>
</div>

<style lang="postcss">
    #card {
        background: #000dff;
        border-radius: 20px;
        transition: transform 100ms ease-out;
        box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
        background: linear-gradient(135deg, #6b73ff, #000dff);
    }
</style>
