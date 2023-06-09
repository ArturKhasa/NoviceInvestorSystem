
<template>
<div>
    <layout>

    <!-- LOADER SCREEN -->
    <LoadingScreen :showLoadingScreen="showLoading" :status="status"></LoadingScreen>

    <!-- EDIT AMOUNT CRYPTO FORM SCREEN -->
    <ModalWindow :showModal="showEditForm">
        <EditCryptoForm @close-edit-form="disableEditCryptoForm" :crypto="cryptoToEdit"></EditCryptoForm>
    </ModalWindow>

    <!-- MAIN -->
    <div class="w-full">
        <div v-if="hasData" class="w-full">
            <div class="grid grid-cols-1">
                <!-- MY PORTFOLIO DATA & CRYPTOS SECTION -->
                <div class="flex flex-col text-white bg-blue-900 w-full mx-auto">

                    <!-- PORTFOLIO INFO -->
                    <div class="mx-auto mt-8 w-11/12 lg:w-10/12 2xl:w-8/12">
                        <div class="w-full">
                            <!-- PORTFOLIO DETAILS TOP -->
                            <div class="flex">
                                <div class="w-6/12 flex flex-wrap">
                                    <h1 class="sm:text-xl font-black lg:text-2xl">Мое портфолио</h1>
                                </div>

                                <div class="w-6/12 flex justify-end items-baseline">
                                    <p class="text-sm sm:text-base mr-2">Общая Сумма: </p>
                                    <p class="sm:text-xl font-black up">
                                        ${{ portfolioTotalWorth }}
                                    </p>
                                </div>
                            </div>

                            <!-- PORTFOLIO DETAILS CONTENT -->
                            <div class="flex mt-2">
                                <div class="flex flex-col w-6/12">
                                    <div class="flex">
                                        <p class="text-xs my-auto sm:text-base">Общее количество активов: </p>
                                        <p class="text-sm font-bold ml-2 sm:text-lg">{{ cryptoData.length }}</p>
                                    </div>

                                    <div class="flex">
                                        <p class="text-xs my-auto sm:text-base">Аккаунт создан: </p>
                                        <p class="text-sm font-bold ml-2 sm:text-lg">{{ portfolio.created_at }}</p>
                                    </div>

                                    <div class="flex">
                                        <p class="text-xs my-auto sm:text-base">Последние изменение: </p>
                                        <p class="text-sm font-bold ml-2 sm:text-lg">{{ portfolio.updated_at }}</p>
                                    </div>
                                </div>

                                <div class="flex flex-col items-end w-6/12">
                                    <div class="flex">
                                        <p class="text-xs my-auto sm:text-base">% Рост активов (24 часа): </p>
                                        <p :class="[(Math.sign(portfolioGrowthPercentage) >= 0)? 'up':'down']"
                                            class="text-sm ml-2 sm:text-lg font-black">
                                                {{ portfolioGrowthPercentage }}%
                                        </p>
                                    </div>

                                    <div class="flex">
                                        <p class="text-xs my-auto sm:text-base">Рост активов (24 часа):</p>
                                        <p :class="[(Math.sign(portfolioGrowth) >= 0)? 'up':'down']"
                                            class="text-sm ml-2 sm:text-lg font-black">
                                            ${{ portfolioGrowth }}
                                        </p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- MY CRYPTOS SECTION -->
                    <div class="bg-white rounded-t-3xl w-full mt-4 text-black align-bottom lg:h-full">
                        <div class="w-11/12 mx-auto mt-5 lg:w-10/12 2xl:w-8/12">
                            <div class="flex justify-between">
                                <div class="">
                                    <h2 class="sm:text-xl font-black lg:text-2xl">Мои активы</h2>
                                </div>
                                <div class="mb-1">
                                    <LinkAddCrypto></LinkAddCrypto>
                                </div>
                            </div>

                            <!-- LIST OF CRYPTOS -->
                            <div class="flex flex-col mt-4 divide-y-2 divide-gray-300 border-b-2 border-t-2 border-gray-300">
                                <div v-for="(crypto, index) in cryptoData" :key="index" class="flex flex-wrap py-2 sm:justify-between lg:justify-start">
                                    <!-- CRYPTO SYMBOL -->
                                    <div class="flex sm:w-3/12">
                                        <img class="w-9 h-9 sm:w-10 sm:h-10 md:w-12 md:h-12" :src="crypto.image">
                                        <div class="flex flex-col ml-2">
                                            <p class="text-sm font-black md:text-lg">{{ crypto.symbol }}</p>
                                            <Link class="underline text-xs font-semibold text-indigo-500 md:text-base" :href="crypto.url">{{ crypto.name }}</Link>
                                        </div>
                                    </div>

                                    <!-- DELETE BUTTON-->
                                    <div class="flex rounded justify-end ml-auto sm:order-6 sm:ml-0 sm:w-1/12 sm:items-center md:ml-auto">
                                        <DeleteCryptoBtn :cg_id="crypto.cg_id" ></DeleteCryptoBtn>
                                    </div>

                                    <!-- ADDED AT -->
                                    <div class="flex items-baseline w-full mt-1 mb-2 sm:my-0 sm:flex-col sm:w-2/12">
                                        <p class="text-xs md:text-base">Добавлено: </p>
                                        <p class="text-sm font-bold ml-2 sm:ml-0 lg:ml-1 md:text-lg">{{ crypto.created_at }}</p>
                                    </div>

                                    <div class="flex w-full sm:w-5/12 justify-between">
                                        <!-- PRICE -->
                                        <div class="flex flex-col sm:w-4/12">
                                            <p class="text-xs md:text-base">Цена: </p>
                                            <p class="text-sm font-bold md:text-lg">${{ crypto.price }}</p>
                                        </div>

                                        <!-- AMOUNT -->
                                        <div class="flex flex-col sm:w-4/12">
                                            <p class="text-xs md:text-base">Количество: </p>
                                            <div class="flex">
                                                <div class="flex">
                                                    <p class="text-sm font-bold md:text-lg">{{ crypto.amount }}</p>
                                                    <p class="text-sm font-bold ml-1 md:text-lg">{{ crypto.symbol }}</p>
                                                    <EditCryptoBtn @edit-crypto="activateEditCryptoForm(crypto)"></EditCryptoBtn>
                                                </div>
                                            </div>
                                        </div>

                                        <!-- TOTAL -->
                                        <div class="flex flex-col items-end sm:w-4/12">
                                            <p class="text-xs md:text-base">Итого: </p>
                                            <p class="text-sm font-bold md:text-lg">${{ crypto.total_worth }}</p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- CHARTS -->
                <div class="bg-white w-full mx-auto">
                    <div class="w-11/12 mx-auto lg:w-10/12 lg:flex lg:flex-row lg:mb-16 2xl:w-8/12">
                        <!-- PORTFOLIO DISTRIBUTION CHART-->
                        <div class="flex flex-col h-40 sm:h-80 lg:w-7/12 xl:w-6/12">
                            <div class="mt-6 lg:mt-8">
                                <h2 class="sm:text-xl font-black lg:text-2xl">
                                    Портфельная Аналитика
                                </h2>
                            </div>
                            <div class="flex w-full mt-4 h-40 sm:mt-6 sm:h-full sm:justify-between lg:justify-start">
                                <div class="w-8/12 sm:h-full sm:w-7/12 lg:w-5/12">
                                    <canvas class="" id="doughnutChart"></canvas>
                                </div>
                                <div class="flex flex-col w-4/12 sm:w-4/12 lg:w-auto lg:ml-3 xl:ml-4">
                                    <div v-for="(c, index) in cryptoDistribution.cryptos" :key="index" class="flex mt-2 md:items-center">
                                        <div class="w-3 h-3 my-auto rounded-full sm:w-5 sm:h-5 md:w-7 md:h-7 lg:w-5 lg:h-5 xl:w-7 xl:h-7" :style="{backgroundColor: chartColors.backgroundColors[index]}"></div>
                                        <p class="text-xs font-bold ml-2 sm:text-sm md:text-base lg:text-sm xl:text-base">{{ c }}</p>
                                        <p class="text-xs font-bold ml-1 sm:text-sm md:text-base lg:text-sm xl:text-base">{{ cryptoDistribution.percentages[index] }}%</p>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- CRYPTOS PERFORMANCE CHART -->
                        <div class="mt-20 sm:mt-24 lg:w-5/12 lg:mt-8 lg:ml-auto xl:w-6/12">
                            <div class="flex flex-wrap">
                                <p class="sm:text-xl font-black lg:text-2xl">
                                    Динамика активов
                                </p>
                                <p class="sm:text-xl font-black sm:ml-1">
                                    (% Последние 7 дней)
                                </p>
                            </div>
                            <div class="mt-5">
                                <canvas class="" id="barChart"></canvas>
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </div>

        <!-- NO DATA -->
        <div v-else class="flex flex-col justify-center items-center w-11/12 mx-auto h-screen">
            <p class="font-extrabold text-lg">
                Сейчас тут пусто.
            </p>

            <p class="font-extrabold text-lg mt-2 text-center">
                Пожалуйста, добавьте активы в ваш портфель.
            </p>

            <div class="mt-5">
                <LinkAddCrypto></LinkAddCrypto>
            </div>
        </div>

    </div>
    </layout>
</div>
</template>
<script>

// Layout
import Layout from '../../Layouts/AppLayout'

// Vue
import { ref, onMounted, watch } from 'vue'

// Services
import { getCryptosById } from '../../Services/cryptoApiService'
import { useToast } from 'vue-toastification'

// COMPONENTS
import { Link } from '@inertiajs/inertia-vue3'
import LinkAddCrypto from '../../Components/LinkAddCrypto.vue'
import ModalWindow from '../../Components/ModalWindow.vue'
import LoadingScreen from '../../Components/LoadingScreen.vue'
import EditCryptoForm from '../../Components/EditCryptoForm.vue'
import EditCryptoBtn from '../../Components/EditCryptoBtn.vue'
import DeleteCryptoBtn from '../../Components/DeleteCryptoBtn.vue'

// Composables
import useEditCryptoForm from '../../Composables/useEditCryptoForm.js'

// Helpers
import { generateCryptoDataArray } from '../../Helpers/GenerateCryptoDataArray'
import getIdsFromArray from '../../Helpers/GetIdsFromArray'
import {
    calculateTotalWorth,
    calculateGrowth,
    calculateGrowthPercentage,
    calculateCryptoDistribution,
    calculateTopCryptos,
} from '../../Helpers/PortfolioHelperFunctions'
import { chartColors } from '../../Charts/ChartColors'

// Charts
import { createDoughnutChart, updateDoughnutChart } from '../../Charts/DoughnutChart.js'
import { createBarChart, updateBarChart } from '../../Charts/BarChart.js'

export default {
  components: {
    Layout,
    Link,
    LinkAddCrypto,
    LoadingScreen,
    ModalWindow,
    DeleteCryptoBtn,
    EditCryptoForm,
    EditCryptoBtn,
  },
  props: {
    portfolio: {
       type: Object,
       required: true
    },
    cryptos: {
       type: Array,
       required: true
    }
  },
  setup(props) {

        const toast = useToast()

        const cryptoData = ref([]);

        let doughnutChart = null;
        let barChart = null;

        let options = {
            created_at: true,
            total_worth: true,
            price_change_24h: true,
            price_change_percentage_7d: true,
        };

        const portfolioTotalWorth = ref(0);
        const portfolioGrowth = ref(0);
        const portfolioGrowthPercentage = ref(0);
        const topCryptos = ref({
            cryptos: [],
            percentage: [],
        });
        const cryptoDistribution = ref({
            cryptos: [],
            percentage: [],
        });

        const status = ref('loading');
        const showLoading = ref(true);
        const hasData = ref(false);

        const { showEditForm, cryptoToEdit, disableEditCryptoForm, activateEditCryptoForm } = useEditCryptoForm();

        const calculatePortfolioData = (cryptosData) => {
            portfolioTotalWorth.value = calculateTotalWorth(cryptosData);
            portfolioGrowth.value = calculateGrowth(cryptosData);
            portfolioGrowthPercentage.value = calculateGrowthPercentage(portfolioTotalWorth.value, portfolioGrowth.value);
        }

        const calculateChartsData = (data) => {
            cryptoDistribution.value = calculateCryptoDistribution(data, portfolioTotalWorth.value);
            topCryptos.value = calculateTopCryptos(data);
        }

        const initCharts = () => {
            const doughnutHtmlElement = document.getElementById("doughnutChart");
            doughnutChart = createDoughnutChart(doughnutHtmlElement, cryptoDistribution.value)

            const barHtmlElement = document.getElementById('barChart');
            barChart = createBarChart(barHtmlElement, topCryptos.value)
        }

        const updateCharts = () => {
            updateDoughnutChart(doughnutChart, cryptoDistribution.value)
            updateBarChart(barChart, topCryptos.value)
        }

        const startWithEmptyData = () => {
            hasData.value = false;
            showLoading.value = false;
            status.value = 'ready';
            cryptoData.value = [];
            calculatePortfolioData(cryptoData.value);
        }

        const fetchCryptoData = async (afterFectch) => {
            try {
                hasData.value = true;
                status.value = 'fetching';

                const ids = getIdsFromArray(props.cryptos)
                const { data } = await getCryptosById(ids)

                status.value = 'ready'
                showLoading.value = false

                cryptoData.value = generateCryptoDataArray(props.cryptos, data, options);

                calculatePortfolioData(cryptoData.value);
                calculateChartsData(cryptoData.value)

                afterFectch()
            }
            catch (ex) {
                status.value = 'ready'
                showLoading.value = false

                if (ex.response && ex.response.status >= 400 && ex.response.status < 500)
                    toast.error(`${ex.response.status} ${ex.response.data}`)
            }
        }

        onMounted(async () => {
            document.title = 'CPT - My portfolio'

            if (props.cryptos.length == 0)
                startWithEmptyData()
            else
                await fetchCryptoData(initCharts)
        });

        watch(() => props.cryptos, async () => {
            disableEditCryptoForm();

            if (props.cryptos.length == 0)
                startWithEmptyData()
            else
                await fetchCryptoData(updateCharts)
        });

        return {
            cryptoData,
            portfolioTotalWorth,
            portfolioGrowth,
            portfolioGrowthPercentage,
            cryptoDistribution,
            chartColors,
            disableEditCryptoForm,
            status,
            showLoading,
            showEditForm,
            cryptoToEdit,
            activateEditCryptoForm,
            hasData,
        }
  },
}
</script>
