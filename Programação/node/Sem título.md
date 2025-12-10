<<<<<<< HEAD
agendamento
=======
  [[Projeto barber]]
>>>>>>> origin/main


![[Pasted image 20251210141718.png]]

<section id="agendamento" className="bg-gradient-to-b from-zinc-900/90 to-black/90 py-16">

        <div className="max-w-7xl mx-auto px-6">

          <div className="grid grid-cols-1 lg:grid-cols-2 gap-10 items-center">

  

            {/* Left - Info */}

            <div className="text-white">

              <h2 className="text-3xl sm:text-4xl font-bold mb-3">Pronto para renovar o visual?</h2>

              <p className="text-zinc-300 mb-6 max-w-xl">

                Selecione o melhor horário para você. Sem filas, sem espera. Nossa equipe está pronta para oferecer um atendimento personalizado.

              </p>

  

              <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">

                <div className="flex items-start gap-3">

                  <div className="w-11 h-11 rounded-lg bg-zinc-800 flex items-center justify-center text-amber-500 shadow">

                    <IconMapPin size={18} />

                  </div>

                  <div>

                    <p className="text-sm text-zinc-300">Endereço</p>

                    <p className="text-sm text-white">Rua Exemplo, 000 - Itarana, ES</p>

                  </div>

                </div>

  

                <div className="flex items-start gap-3">

                  <div className="w-11 h-11 rounded-lg bg-zinc-800 flex items-center justify-center text-amber-500 shadow">

                    <IconPhone size={18} />

                  </div>

                  <div>

                    <p className="text-sm text-zinc-300">Telefone</p>

                    <p className="text-sm text-white">(27) 99999-8888</p>

                  </div>

                </div>

  

                <div className="flex items-start gap-3">

                  <div className="w-11 h-11 rounded-lg bg-zinc-800 flex items-center justify-center text-amber-500 shadow">

                    <IconClock size={18} />

                  </div>

                  <div>

                    <p className="text-sm text-zinc-300">Horário</p>

                    <p className="text-sm text-white">Seg - Sáb: 09:00 - 20:00</p>

                  </div>

                </div>

  

                <div className="flex items-start gap-3">

                  <div className="w-11 h-11 rounded-lg bg-zinc-800 flex items-center justify-center text-amber-500 shadow">

                    <IconCalendarEvent size={18} />

                  </div>

                  <div>

                    <p className="text-sm text-zinc-300">Agendamento</p>

                    <p className="text-sm text-white">Reserve online em poucos minutos</p>

                  </div>

                </div>

              </div>

  

              <div className="mt-8">

                <a href="#agendamento" className="inline-block bg-amber-600 hover:bg-amber-700 text-white px-5 py-3 rounded-md font-semibold">

                  AGENDAR AGORA

                </a>

              </div>

            </div>

  

            {/* Right - Form */}

            <div>

              <div className="bg-zinc-900 p-6 sm:p-8 rounded-xl shadow-xl border border-zinc-800">

                <h3 className="text-xl text-white font-semibold mb-4 flex items-center gap-3">

                  <span className="inline-block text-amber-500"><IconCalendarEvent size={22} /></span>

                  Faça seu Agendamento

                </h3>

  

                <form className="space-y-3">

                  <input

                    type="text"

                    placeholder="Nome completo"

                    className="w-full px-4 py-3 bg-zinc-800 text-white rounded-md border border-zinc-700 focus:outline-none focus:border-amber-500"

                  />

  

                  <input

                    type="tel"

                    placeholder="Telefone"

                    className="w-full px-4 py-3 bg-zinc-800 text-white rounded-md border border-zinc-700 focus:outline-none focus:border-amber-500"

                  />

  

                  <select className="w-full px-4 py-3 bg-zinc-800 text-white rounded-md border border-zinc-700 focus:outline-none focus:border-amber-500">

                    <option>Selecione o serviço</option>

                    <option>Corte Clássico - R$50</option>

                    <option>Barba Completa - R$40</option>

                    <option>Combo Corte + Barba - R$80</option>

                    <option>Corte Moderno - R$60</option>

                  </select>

  

                  <div className="grid grid-cols-1 sm:grid-cols-2 gap-3">

                    <input type="date" className="w-full px-4 py-3 bg-zinc-800 text-white rounded-md border border-zinc-700 focus:outline-none focus:border-amber-500" />

                    <input type="time" className="w-full px-4 py-3 bg-zinc-800 text-white rounded-md border border-zinc-700 focus:outline-none focus:border-amber-500" />

                  </div>

  

                  <textarea placeholder="Observações (opcional)" className="w-full px-4 py-3 bg-zinc-800 text-white rounded-md border border-zinc-700 focus:outline-none focus:border-amber-500"></textarea>

  

                  <button className="w-full bg-amber-600 hover:bg-amber-700 text-white font-semibold py-3 rounded-md transition">AGENDAR</button>

                </form>

              </div>

            </div>

          </div>

        </div>

      </section>