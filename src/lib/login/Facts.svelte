<script>
    import {fly} from "svelte/transition";

    const facts = [
        {
            title: "暴力或隐蔽作弊 - 由您决定！",
            description: "LiquidBounce 内置多种暴力和隐蔽作弊功能，是完美的全能客户端。",
        },
        {
            title: "多版本支持",
            description: "通过集成的版本切换器，您可以轻松在不同 Minecraft 版本之间切换，无需重启游戏！",
        },
        {
            title: "多服务器支持",
            description: "LiquidBounce 为许多不同的服务器提供功能和绕过方案。"
        },
        {
            title: "自动配置系统",
            description: "LiquidBounce 的自动配置系统会在加入知名服务器时自动为您应用最佳设置。",
        },
        {
            title: "可自定义",
            description: "LiquidBounce 的 UI 完全可自定义。您可以按自己的喜好更改整个界面。",
        },
        {
            title: "600 万总下载量",
            description:
                "LiquidBounce 是有史以来最受欢迎的作弊客户端之一。",
        },
        {
            title: "免费开源",
            description: "LiquidBounce 的源代码是公开的。",
        },
        {
            title: "脚本 API",
            description:
                "LiquidBounce 的脚本 API 允许用户编写自己的模块和命令。",
        },
    ];

    let currentFact = 0;

    function handlePrevClick(e) {
        if (currentFact <= 0) {
            currentFact = facts.length - 1;
        } else {
            currentFact--;
        }
    }

    function handleNextClick(e) {
        if (currentFact >= facts.length - 1) {
            currentFact = 0;
        } else {
            currentFact++;
        }
    }

    setInterval(() => {
        handleNextClick();
    }, 5000);
</script>

<div class="wrapper">
    {#key currentFact}
        <div class="fact" in:fly={{duration: 200, x: 50}} out:fly={{duration: 200, x: -50}}>
            <div class="title">{facts[currentFact].title}</div>
            <div class="description">{facts[currentFact].description}</div>
            <div class="buttons-wrapper">
                <button
                    type="button"
                    class="button-switch-fact"
                    on:click={handlePrevClick}
                >
                    <img src="img/icon/icon-prev.svg" alt="prev" />
                </button>
                <button
                    type="button"
                    class="button-switch-fact"
                    on:click={handleNextClick}
                >
                    <img src="img/icon/icon-next.svg" alt="next" />
                </button>
            </div>
        </div>
    {/key}
</div>

<style>
    .wrapper {
        position: relative;
    }

    .fact {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        width: 400px;
        display: flex;
        flex-direction: column;
        row-gap: 10px;
    }

    .title {
        font-size: 40px;
        color: white;
        font-weight: 600;
    }

    .description {
        font-size: 18px;
        color: rgba(255, 255, 255, 0.5);
    }

    .buttons-wrapper {
        display: flex;
        column-gap: 10px;
    }

    .button-switch-fact {
        height: 44px;
        width: 44px;
        border-radius: 50%;
        border: solid 2px rgba(255, 255, 255, 0.5);
        background-color: transparent;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: ease background-color 0.2s;
        cursor: pointer;
    }

    .button-switch-fact:hover {
        background-color: rgba(255, 255, 255, 0.1);
    }
</style>
