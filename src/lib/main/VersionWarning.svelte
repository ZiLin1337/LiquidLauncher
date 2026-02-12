<script>
    import { createEventDispatcher } from "svelte";
    import SettingsContainer from "../settings/SettingsContainer.svelte";
    import Description from "../settings/Description.svelte";
    import ButtonSetting from "../settings/ButtonSetting.svelte";

    export let launchVersionWarningCountdown;

    const dispatch = createEventDispatcher();
</script>

<SettingsContainer
        title="您即将启动一个不受支持的版本！"
        on:hideSettings={() => dispatch('hide')}
>
    <Description
            description="所选版本的 LiquidBounce 已不再受官方支持。我们建议升级到最新版本的 LiquidBounce Nextgen，它支持从 1.7 开始的所有 Minecraft 版本。"
    />
    <ButtonSetting
            text="立即切换到 Nextgen"
            on:click={() => dispatch('switchToNextgen')}
            color="#4677FF"
    />
    <ButtonSetting
            disabled={launchVersionWarningCountdown > 0}
            text="仍要启动{launchVersionWarningCountdown > 0 ? ` (${launchVersionWarningCountdown})` : ''}"
            on:click={() => dispatch('runClientAnyway')}
            color="#B83529"
    />
</SettingsContainer>