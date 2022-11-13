<script>
    import CommonHelper from "@/utils/CommonHelper";
    import Field from "@/components/base/Field.svelte";
    import { slide } from "svelte/transition";

    export let key = "";
    export let options = {};

    export let fieldName = "";
    export let collection = {};

    let showFiltersInfo = false;

    // load defaults
    $: if (CommonHelper.isEmpty(options)) {
        options = {
            clause: "",
        };
    }
</script>

<div class="grid">
    <div class="col-sm-6">
        <Field class="form-field required" name="schema.{key}.options.clause" let:uniqueId>
            <label for={uniqueId}>Clause</label>
            <input type="text" id={uniqueId} required bind:value={options.clause} />
        </Field>
    </div>
    <div class="col-sm-6">
        <span
            class="expand-handle txt-sm txt-bold txt-nowrap link-hint"
            on:click={() => (showFiltersInfo = !showFiltersInfo)}
        >
            {showFiltersInfo ? "Hide available fields" : "Show available fields"}
        </span>
    </div>
    {#if showFiltersInfo}
        <div transition:slide|local={{ duration: 150 }}>
            <div class="alert alert-info m-0">
                <div class="content">
                    <p class="m-b-0">The following record fields are available:</p>
                    <div class="inline-flex flex-gap-5">
                        <code>id</code>
                        <code>created</code>
                        <code>updated</code>
                        {#each collection.schema.filter((field) => field.name !== fieldName) as field}
                            {#if field.type === "relation" || field.type === "user"}
                                <code>{field.name}.*</code>
                            {:else}
                                <code>{field.name}</code>
                            {/if}
                        {/each}
                    </div>
                </div>
            </div>
        </div>
    {/if}
</div>
