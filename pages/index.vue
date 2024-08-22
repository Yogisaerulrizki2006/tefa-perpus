<template>
    <div class="container-fluid">
        <div class="row my-5">
            <div class="col-lg-6">
                <nuxt-link to="/pengunjung/tambah">
                    <div class="card bg-pengunjung rounded-5">
                        <div class="card-body">
                            <h2 class="">pengunjung</h2>
                        </div>
                    </div>
                </nuxt-link>
            </div>
            <div class="col-lg-6">
                <nuxt-link to="/buku">
                    <div class="card bg-buku rounded-5">
                        <div class="card-body">
                            <h2>cari buku</h2>
                        </div>
                    </div>
                </nuxt-link>
            </div>
            <h2 class="mt-5" style="font-style: margin-left 100px;">STATISTIC</h2>
            <div class="col-lg-6">
                <div class="card statistic rounded-5">
                    <div class="card-body">
                        <h1>{{ visitor }} pengunjung</h1>
                    </div>
                </div>
            </div>
            <div class="col-lg-6">
                <div class="card bg-statistic rounded-5">
                    <div class="card-body">
                        <h1>{{ book }}BUKU</h1>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const book = ref(0)
const visitor = ref(0)

const hitungDatabuku = async () => {
    const { data, count } = await supabase.from("buku").select("*", { count: 'exact' })
    if (data) book.value = count

}
const hitungData = async () => {
    const { data, count } = await supabase.from("pengunjung").select("*", { count: 'exact' })
    if (data) visitor.value = count

}
onMounted(() => {
    hitungDatabuku()
    hitungData()
})
</script>

<style scoped>
.card {
    height: 250px;
    box-shadow: 1px 1px 10px #030303;
}

.card.bg-pengunjung {
    background-image: url('../assets/img/bg-home-kunjungan.jpeg');
    background-repeat: no-repeat;
    background-position: center center;
    background-size: cover;
    -webkit-text-fill-color: #030303;
}

.card.bg-buku {
    background: url('../assets/img/bg-home-cari-buku.jpg');
    background-size: cover;
    -webkit-text-fill-color: #030303;
}

.card.statistic {
    background-repeat: no-repeat;
    background-position: center center;
    background-color: #ff5100;
}

.card.bg-statistic {
    background-color: hsl(209, 99%, 29%);
    background-repeat: no-repeat;
}

.chart {

    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 100px;
}

h1 {
    margin-top: 80px;
}
</style>
