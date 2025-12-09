import Link from "next/link"

import SvgIcon from "../components/Logo"

import BotaoIcone from "../components/BotaoIcone"

import { IconRazorElectric } from "../assets/icone"

  
  

const BarraSuperior = () => {

    return(

        <nav className="w-full bg-black/80 backdrop-blur-md sticky top-0 z-50">

            <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 sm:py-6">

                <div className="flex items-center justify-between">

                    <div className="flex-shrink-0">

                        <SvgIcon/>

                    </div>

  

                    <div className="hidden md:flex flex-row gap-8 lg:gap-12">

                        <Link href="#inicio" className="text-white hover:text-amber-500 transition-colors text-sm lg:text-base"> Inicio </Link>

                        <Link href="/" className="text-white hover:text-amber-500 transition-colors text-sm lg:text-base"> Quem Somos </Link>

                        <Link href="/" className="text-white hover:text-amber-500 transition-colors text-sm lg:text-base"> Contato </Link>

                    </div>

  

                    <div className="ml-auto">

                        <BotaoIcone icone={(

                            <IconRazorElectric aria-label="Icone de Maquina" size={20} />

                        )}> Agendar

                        </BotaoIcone>

                    </div>

                </div>

                <div className="md:hidden flex flex-col gap-3 mt-4 pt-4 border-t border-gray-700">

                    <Link href="#inicio" className="text-white text-sm hover:text-amber-500 transition-colors"> Inicio </Link>

                    <Link href="/" className="text-white text-sm hover:text-amber-500 transition-colors"> Quem Somos </Link>

                    <Link href="/" className="text-white text-sm hover:text-amber-500 transition-colors"> Contato </Link>

                </div>

            </div>

        </nav>

    )  

}

  

export default BarraSuperior