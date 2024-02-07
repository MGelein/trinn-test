<script>
  import { onMount } from "svelte";

  const ID = "trb1914-having-fun";
  let remote, controller;

  onMount(async () => {
    const { TRINNConfig, TRINNController, TRINNRemote } = await import(
      "trinn-remote-control"
    );
    TRINNConfig.secure = true;
    TRINNConfig.host = "peerjs.exploretriple.com";
    controller = new TRINNController(ID);
    remote = new TRINNRemote(ID);

    remote.onData((data) => console.log({ data: data }));
    controller.onData((data) => console.log({ data: data }));
  });

  const controllerSend = () => {
    controller.sendData({ from: "controller", to: "remote" });
  };

  const remoteSend = () => {
    remote.sendData({ from: "remote", to: "controller" });
  };
</script>

<button on:click={controllerSend}>Send to Remote</button>
<button on:click={remoteSend}>Send to Controller</button>
