<script setup>
import axios from 'axios'
import { ref, computed } from 'vue'

const props = defineProps({
    PageSection: String,
    Up: Boolean
})

const PageSectionRef = ref(props.PageSection)
const uptimeData = ref(null)
const name = ref("")
const up = ref(props.Up)

axios.get(`https://legaldeskspeedtest-production.up.railway.app/api/v1/uptime?section=${PageSectionRef.value}`)
  .then(response => {
    uptimeData.value = response.data
  })
  .catch(error => {
    console.error(error)
})

if (PageSectionRef.value == "front_page") {
    name.value = "Front Page"
} else if (PageSectionRef.value == "product_page") {
    name.value = "Product Page"
} else if (PageSectionRef.value == "wizard") {
    name.value = "Wizard"
} else if (PageSectionRef.value == "login") {
    name.value = "Login"
} else if (PageSectionRef.value == "basket") {
    name.value = "Basket"
}

const computedClass = computed(() => {
  if (uptimeData.value === null) {
    return ''
  } else {
    return up.value ? 'container-down' : 'container-up'
  }
})

</script>

<template>
    <div class="container" :class="computedClass">
        <span v-if="uptimeData != null">
            <div class="title">{{ name }}</div>
            <div class="value">
                <span class="number">{{ uptimeData["uptime"].toFixed(2) }}</span>
                <span class="percent">%</span>
            </div>
        </span>
    </div>
</template>



<style scoped>
    .container {
        border: 1px solid rgb(192, 192, 192);
        background-color: rgb(224, 224, 224);
        float:left;
        margin: 0 auto;
        width: 170px;
        height: 170px;
        /* rounded corners */
        -webkit-border-radius: 20px;
        /* drop shadow */
        -webkit-box-shadow: 0px 5px 10px rgba(0, 0, 0, 0.2);
    }

    @media screen and (max-width: 1200px) {
        .container {
            width: 100%;
            height: 100px;
            margin-bottom: 10px;
        }
    }

    .container-up{
        background-color: rgb(170, 255, 188);}
    .container-down {
        background-color: rgb(255, 170, 170);
    }
    .title {
        padding-top: 20px;
        text-align: center;
        font-size: 1.5rem;
    }

    .value {
        padding-top: 20px;
        text-align: center;
    }

    .number {
        font-size: 2rem;
    }

    @media screen and (max-width: 1200px) {
        .number {
            font-size: 1.2rem;
        }
    }
    .percent {
        font-size: 1.2rem;
    }
</style>
