<script lang="ts">
    import {PrivateKey} from '@bsv/sdk';

    let WIF: string;
    let address: string;
    let keyGenerated: false;

    function createKeys () {
        const privKey= PrivateKey.fromRandom();
        WIF = privKey.toWif();
        console.log("Private Key WIF format is ", WIF);
        address = privKey.toAddress();
        console.log("Your bitcoin address is ", address);
    }

    function copyWIF() {
        var copyText = `Private Key WIF format is ${WIF}, Your bitcoin address is ${address}`;
        navigator.clipboard.writeText(copyText);
        console.log("Text Copied!");
    }
</script>

<section>
   <div>
    <button class="bg-black text-white p-2 rounded mx-auto" on:click={createKeys}> Generate New Wallet Keys </button>

    <div>
    <p>Private Key WIF format is {WIF} </p>
    </div>

    <div>
    <p>Your Bitcoin Address is {address} </p>
    </div>

    {#if keyGenerated}
    <div class="flex flex-row-reverse"><button class="bg-gray-400 text-gray-700 shadow hover:shadow-md hover:text-slate-200 px-3 py-2 rounded"
    on:click={() => copyWIF()}>Copy</button>
    </div>
    {/if}
   </div>
</section>-