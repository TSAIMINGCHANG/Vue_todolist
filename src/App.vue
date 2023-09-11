<script setup>
    import DolistItem from "@/component/Item.vue"
    import { ref,reactive,onBeforeMount}from "vue"
    import{ v4 as uuidv4 } from "uuid"
    const dolist = ref("")
    const dolists =reactive([])
    const StorageKey = "DO-List"
    const save =(key,data) => {
        localStorage.setItem(key, JSON.stringify(data))
    }
    const addlist = () =>{
        if(dolist.value != "") {
            const item ={
                id: uuidv4(),
                list: dolist.value,
                completed: false,
            }
            dolists.unshift(item)
            save(StorageKey, dolists)
            dolist.value=""
            console.log(item)
        }   
    }
    const removeItem = (id) => {
        const index = dolists.findIndex((dolist) => {
            return dolist.id === id
        })
        dolists.splice(index,1)
        save(StorageKey,dolists)
    }
   onBeforeMount(() => {
    const savedolists =JSON.parse(localStorage.getItem(StorageKey))
    console.log(savedolists)
    if ((savedolists != null) && (savedolists.length > 0))
   //if (savedolists.length > 0 )
        {
        dolists.push(...savedolists)
        }
   })
    
</script>

<template>
    <main class="container mx-auto">
      <header class="m-2">
        <h1 class="text-6xl font-thin select-none">TODO!</h1>
        <div class="font-semibold select-none text-neutral-600">simple and studid todo app</div>
      </header>
      <form class="px-10 py-12 bg-white shadow-sm">
        <section class="flex">
          <input v-model="dolist" type="text" placeholder="做點重要的事吧..." class="w-full text-2xl focus:outline-none input-lg input input-bordered" />
          <button @click.prevent="addlist" class="text-xl btn-lg btn btn-neutral">新增</button>
        </section>
      </form>
      <ul>
      <DolistItem
        @remove-auo-item="removeItem"
        v-for="d in dolists"
        :dolist="d"
      />
    </ul>

    </main>


</template>

<style scoped></style>
