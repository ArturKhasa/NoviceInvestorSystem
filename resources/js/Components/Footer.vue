<template>
    <footer class="bg-gray-800 mt-8">
                <div class="flex flex-col w-10/12 mx-auto py-8 text-white">
                    <!-- LINKS -->
                    <div class="flex flex-col mx-auto">
                        <div class=" flex w-full mb-1 justify-center">
                            <p class="font-bold">Разработано:</p>
                        </div>

                        <div class="flex">
                            <div class="flex items-baseline font-bold">
                                <p class="flex items-center text-sm sm:text-base">
                                    <font-awesome-icon :icon="['fas', 'envelope']" class="mr-2"/>
                                    arturbk2@yandex.ru
                                </p>
                            </div>
                        </div>

                        <div class="flex justify-between mt-2 mx-auto text-lg font-black">
                            <div class="flex ml-3">
                                <a href="https://github.com/ArturKhasa">
                                    <font-awesome-icon :icon="['fab', 'github']" size="lg" />
                                </a>
                            </div>
                        </div>
                    </div>

                    <!-- INFO -->
                    <div class="flex flex-col mt-3 mx-auto items-center text-center">
                        <div class="flex flex-wrap justify-center font-bold">
                            <div class="flex flex-wrap items-baseline">
                                <font-awesome-icon :icon="['fas', 'cog']" size="lg" class="my-auto"/>
                            </div>
                            <p class="ml-1">Хасаханов Артур</p>
                        </div>
                        <p class="font-bold mt-2">
                            Платформа начинающего крипто инвестора
                        </p>
                        <p class="font-bold">
                            2023
                        </p>
                    </div>

                </div>
            </footer>
</template>

<script>
export default {
    setup() {

    },
}
</script>
