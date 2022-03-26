<template>
  <div>
    <div class="layout flex min-h-screen items-center justify-center bg-teal-500">
      <div class="wrapper bg-white shadow-md rounded-md w-96">
        <header
          class="flex text-xl font-bold p-4 text-teal-500 items-center border-b border-b-slate-400"
        >
          <i class="bx bx-left-arrow-alt text-lg cursor-pointer mr-2"></i>Погода
        </header>
        <section class="input-part m-8">
          <p class="info-txt hidden text-center rounded-md text-md mb-4 py-4 px-2"></p>
          <div class="content">
            <input
              type="text"
              class="w-full h-14 rounded-md text-md text-center px-4 border border-slate-300 placeholder:text-slate-400 focus:border-2 focus:border-teal-500 valid:border-2 valid:border-teal-500"
              spellcheck="false"
              placeholder="Enter city name"
              required
            />
            <div
              class="separator h-px w-full flex items-center justify-center relative bg-slate-400 my-8 before:content-['or'] before:text-slate-400 before:px-4 before:bg-white before:text-lg"
            ></div>
            <button
              class="text-white h-14 rounded-md text-md bg-teal-500 hover:bg-teal-700 w-full"
            >Get Device Location</button>
          </div>
        </section>
        <section class="weather-part hidden items-center justify-center flex-col mt-5">
          <!--<img class="max-w-xs" src alt="Weather Icon" />-->
          <div class="temp flex font-medium text-7xl">
            <span class="numb font-semibold">_</span>
            <span class="deg block text-4xl mt-2 mr-1">°</span>C
          </div>
          <div class="weather mt-0 mb-8 mx-10 text-lg text-center">_ _</div>
          <div class="flex text-lg px-8 text-center mb-8 justify-start">
            <i class="bx bx-map text-lg mt-4 mr-4"></i>
            <span>_, _</span>
          </div>
          <div class="bottom-details flex w-full justify-between border-t border-t-slate-300">
            <div class="column feels flex w-full items-center justify-center py-4">
              <i class="bx bxs-thermometer text-lg text-teal-500"></i>
              <div class="details ml-1">
                <div class="temp flex font-medium text-lg">
                  <span class="numb-2">_</span>
                  <span class="deg m-0 text-sm p-1">°</span>C
                </div>
                <p class="mt-0">Ощущается</p>
              </div>
            </div>
            <div
              class="column humidity flex w-full items-center justify-center py-4 border-l border-l-slate-300"
            >
              <i class="bx bxs-droplet-half text-lg text-teal-500"></i>
              <div class="details ml-1">
                <span class="font-medium text-lg mt-0">_</span>
                <p class="mt-0">Влажность</p>
              </div>
            </div>
          </div>
        </section>
      </div>
    </div>
  </div>
</template>

<style scoped>
.wrapper.active header i {
  margin-left: 5px;
  font-size: 30px;
}
.wrapper.active .input-part {
  display: none;
}
.input-part .info-txt.error {
  color: #721c24;
  display: block;
  background: #f8d7da;
  border: 1px solid #f5c6cb;
}
.input-part .info-txt.pending {
  color: #0c5460;
  display: block;
  background: #d1ecf1;
  border: 1px solid #bee5eb;
}
.wrapper.active .weather-part {
  display: flex;
}
</style>