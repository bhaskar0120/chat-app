<script context="module">
    export async function load(cont){
        let group = cont.page.params.group;
        return {props: {page:group}};
    }

</script>
<script>
    import GroupSider from './main/_groupSider.svelte';
    export let page;

</script>
{page}
<GroupSider data={[]} showB={false}/>