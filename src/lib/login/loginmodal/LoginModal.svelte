<script>
    import ModalButton from "./ModalButton.svelte";
    import ModalInput from "./ModalInput.svelte";

    import {invoke} from "@tauri-apps/api/core";
    import {listen} from "@tauri-apps/api/event";
    import {openUrl} from "@tauri-apps/plugin-opener";

    export let options;

    let offlineUsername;
    let microsoftCode = null;

    async function handleOfflineLoginClick(e) {
        if (offlineUsername.length > 16 || offlineUsername.length < 1) {
            alert("用户名长度必须在 1 到 16 个字符之间。");
            return;
        }

        const usernameRegex = /^[a-zA-Z0-9_]+$/;
        if (!usernameRegex.test(offlineUsername)) {
            alert("用户名只能包含字母、数字和下划线。");
            return;
        }

        options.start.account = await invoke("login_offline", {username: offlineUsername});
        options.store();
    }

    async function handleMicrosoftLoginClick(e) {
        try {
            options.start.account = await invoke("login_microsoft");
            options.store();
        } catch (err) {
            alert(
                "微软认证失败。\n\n" +
                 err + "\n\n" +
                "如果无法解决此问题，请使用"离线"登录选项，" +
                "并尝试通过客户端内置的账户管理器登录。"
            );
            cancelMicrosoft();
        }
    }

    listen("microsoft_code", (e) => {
        microsoftCode = e.payload;
    });

    function cancelMicrosoft() {
        microsoftCode = null;
    }
</script>

<div class="modal">
    {#if !microsoftCode}
        <div class="title">登录</div>

        <ModalInput placeholder="用户名" icon="person" characterLimit={16} bind:value={offlineUsername} />
        <ModalButton text="离线登录" primary={false} on:click={handleOfflineLoginClick} />
        <ModalButton text="微软登录" primary={true} on:click={handleMicrosoftLoginClick} />
    {:else}
        <div class="title">微软登录</div>

        <ModalInput placeholder="微软代码" characterLimit={16} icon="lock" bind:value={microsoftCode} />
        <ModalButton text="链接" primary={true} on:click={() => openUrl("https://microsoft.com/link")} />
        <ModalButton text="取消" primary={false} on:click={cancelMicrosoft} />
    {/if}
</div>

<style>
    .modal {
        background-color: rgba(0, 0, 0, 0.26);
        padding: 30px;
        border-radius: 12px;
        width: 320px;
        display: flex;
        flex-direction: column;
        row-gap: 15px;
    }

    .title {
        color: white;
        font-size: 22px;
        margin: 0 auto;
        position: relative;
        width: max-content;
        margin-bottom: 40px;
    }

    .title::after {
        content: "";
        position: absolute;
        height: 5px;
        width: calc(100% - 10px);
        left: 50%;
        bottom: -20px;
        transform: translateX(-50%);
        background-color: #4677FF;
        border-radius: 5px;
    }
</style>