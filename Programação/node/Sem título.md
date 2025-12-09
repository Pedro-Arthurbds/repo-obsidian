  

import Image from "next/image";

import BarraSuperior from "./widgets/BarraSuperior";

  

export default function Home() {

  
  

    return (

      <div>

        <BarraSuperior/>

        <section className="relative w-full min-h-screen flex items-center justify-center overflow-hidden">

        <Image

          src="/image.jpg"

          fill

          alt="Background"

          className="object-cover"

          priority

        />

  

        <div className="absolute inset-0 bg-gradient-to-r from-black/70 via-black/60 to-black/40"></div>

  

        <div className="relative z-10 max-w-6xl w-full px-4 sm:px-6 lg:px-8 py-12 sm:py-16 lg:py-0">

          <div className="max-w-2xl">

            <span className="text-amber-400 font-semibold tracking-widest text-xs sm:text-sm lg:text-base uppercase block mb-3 sm:mb-4">

              Desde 2015

            </span>

  

            <h1 className="text-white text-3xl sm:text-4xl md:text-5xl lg:text-6xl font-bold leading-tight mb-4 sm:mb-6">

              Estilo Clássico,<br className="hidden sm:block" />

              Corte <span className="text-amber-400">Moderno.</span>

            </h1>

  

            <p className="text-gray-300 text-sm sm:text-base lg:text-lg max-w-xl leading-relaxed mb-6 sm:mb-8">

              Mais do que um corte de cabelo, uma experiência de cavalheiros. Relaxe, tome uma cerveja gelada e deixe nossos mestres cuidarem do seu visual.

            </p>

  

            <div className="flex flex-col sm:flex-row gap-3 sm:gap-4">

              <button className="bg-amber-600 hover:bg-amber-700 text-white font-semibold py-2.5 sm:py-3 px-4 sm:px-6 rounded-md transition text-sm sm:text-base w-full sm:w-auto">

                AGENDAR HORÁRIO

              </button>

              <button className="bg-purple-600 hover:bg-purple-700 text-white font-semibold py-2.5 sm:py-3 px-4 sm:px-6 rounded-md transition text-sm sm:text-base w-full sm:w-auto">

                DESCUBRA SEU ESTILO

              </button>

            </div>

          </div>

        </div>

        </section>

  

        <section>

  

          <div>

  

            <p>lorem</p>

  

          </div>

  

        </section>

  

      </div>

    );

}