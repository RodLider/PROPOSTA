
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerador de Propostas Profissional</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f1f5f9;
        }

        /* Estilos de Impressão (mantidos para caso o usuário use o atalho do sistema Ctrl+P) */
        @media print {
            body {
                background: white !important;
                padding: 0 !important;
                margin: 0 !important;
            }
            
            .no-print {
                display: none !important;
            }

            .print-area {
                box-shadow: none !important;
                border: none !important;
                width: 100% !important;
                max-width: 100% !important;
                margin: 0 !important;
                padding: 1cm !important;
            }

            .bg-slate-900 {
                background-color: #0f172a !important;
                color: white !important;
                -webkit-print-color-adjust: exact;
            }

            .bg-blue-50 {
                background-color: #eff6ff !important;
                -webkit-print-color-adjust: exact;
            }

            .bg-slate-50 {
                background-color: #f8fafc !important;
                -webkit-print-color-adjust: exact;
            }

            @page {
                margin: 1cm;
            }
        }

        .fade-in {
            animation: fadeIn 0.4s ease-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body class="p-4 md:p-8">

    <div id="main-container" class="max-w-4xl mx-auto">
        
        <!-- TELA DE FORMULÁRIO -->
        <div id="view-form" class="fade-in">
            <header class="mb-8">
                <h1 class="text-3xl font-black text-slate-800 flex items-center gap-2">
                    <i data-lucide="file-text" class="text-blue-600"></i>
                    Nova Proposta
                </h1>
                <p class="text-slate-500">Insira os dados para gerar o resumo da proposta.</p>
            </header>

            <form id="propostaForm" class="bg-white p-6 md:p-10 rounded-3xl shadow-xl border border-slate-200 space-y-6">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <!-- Nome -->
                    <div class="space-y-1">
                        <label class="text-xs font-bold text-slate-500 uppercase ml-1">Nome do Cliente</label>
                        <input type="text" name="nome" required placeholder="Nome completo"
                            class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none">
                    </div>

                    <!-- CPF -->
                    <div class="space-y-1">
                        <label class="text-xs font-bold text-slate-500 uppercase ml-1">CPF</label>
                        <input type="text" name="cpf" id="cpfInput" required placeholder="000.000.000-00" maxlength="14"
                            class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none">
                    </div>

                    <!-- Categoria -->
                    <div class="space-y-1">
                        <label class="text-xs font-bold text-slate-500 uppercase ml-1">Finalidade</label>
                        <select name="categoria" required
                            class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none cursor-pointer">
                            <option value="IMÓVEL">IMÓVEL</option>
                            <option value="REFORMA">REFORMA</option>
                            <option value="CONSTRUÇÃO">CONSTRUÇÃO</option>
                            <option value="VEÍCULOS">VEÍCULOS</option>
                            <option value="RURAL">RURAL</option>
                            <option value="CAPITAL DE GIRO">CAPITAL DE GIRO</option>
                        </select>
                    </div>

                    <!-- Preço do Bem -->
                    <div class="space-y-1">
                        <label class="text-xs font-bold text-slate-500 uppercase ml-1">Preço do Bem</label>
                        <input type="text" name="preco" required placeholder="R$ 0,00"
                            class="money w-full px-4 py-3 bg-blue-50 border border-blue-100 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none font-bold text-blue-700">
                    </div>

                    <!-- Parcela -->
                    <div class="space-y-1">
                        <label class="text-xs font-bold text-slate-500 uppercase ml-1">Valor da Parcela</label>
                        <input type="text" name="parcela" required placeholder="R$ 0,00"
                            class="money w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none">
                    </div>

                    <!-- Entrada -->
                    <div class="space-y-1">
                        <label class="text-xs font-bold text-slate-500 uppercase ml-1">1ª Parcela (Entrada)</label>
                        <input type="text" name="entrada" required placeholder="R$ 0,00"
                            class="money w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none">
                    </div>
                </div>

                <button type="submit" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-4 rounded-2xl transition-all shadow-lg shadow-blue-200 uppercase tracking-widest flex items-center justify-center gap-2">
                    <i data-lucide="arrow-right-circle"></i>
                    Gerar Proposta Detalhada
                </button>
            </form>
        </div>

        <!-- TELA DE VISUALIZAÇÃO -->
        <div id="view-print" class="hidden fade-in">
            <div class="no-print flex justify-start mb-6">
                <button onclick="toggleView('form')" class="flex items-center gap-2 text-slate-500 hover:text-blue-600 font-bold transition-colors">
                    <i data-lucide="chevron-left"></i> Voltar para Edição
                </button>
            </div>

            <div class="print-area bg-white rounded-3xl shadow-2xl overflow-hidden border border-slate-200">
                <!-- Header do Documento -->
                <div class="bg-slate-900 p-10 text-white flex flex-col md:flex-row justify-between items-center gap-4">
                    <div class="text-center md:text-left">
                        <h2 class="text-2xl font-black tracking-tighter uppercase">Resumo da Proposta de Crédito</h2>
                        <p class="text-slate-400 text-sm mt-1" id="out-data"></p>
                    </div>
                    <div class="text-center md:text-right">
                        <div class="bg-blue-600 text-white px-4 py-1 rounded-full text-[10px] font-black tracking-widest uppercase">Documento Gerado</div>
                    </div>
                </div>

                <div class="p-10 md:p-16 space-y-12">
                    <!-- Dados do Cliente -->
                    <section>
                        <h3 class="text-xs font-black text-slate-400 uppercase tracking-widest border-b border-slate-100 pb-2 mb-6">Informações da Proposta</h3>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                            <div>
                                <label class="text-[10px] text-slate-400 font-bold uppercase block mb-1">Nome Completo</label>
                                <p class="text-xl font-bold text-slate-800 uppercase" id="out-nome"></p>
                            </div>
                            <div>
                                <label class="text-[10px] text-slate-400 font-bold uppercase block mb-1">CPF Registrado</label>
                                <p class="text-xl font-bold text-slate-800" id="out-cpf"></p>
                            </div>
                        </div>
                    </section>

                    <!-- Detalhes do Bem -->
                    <section>
                        <h3 class="text-xs font-black text-slate-400 uppercase tracking-widest border-b border-slate-100 pb-2 mb-6">Especificação do Bem</h3>
                        <div class="bg-slate-50 p-6 rounded-2xl flex items-center gap-4 border border-slate-100">
                            <div class="bg-white p-4 rounded-xl shadow-sm text-blue-600" id="out-icon-box">
                                <i data-lucide="file-check"></i>
                            </div>
                            <div>
                                <p class="text-2xl font-black text-slate-900" id="out-categoria"></p>
                                <p class="text-xs text-slate-500 font-medium uppercase tracking-wider">Modalidade de Crédito</p>
                            </div>
                        </div>
                    </section>

                    <!-- Financeiro -->
                    <section>
                        <h3 class="text-xs font-black text-slate-400 uppercase tracking-widest border-b border-slate-100 pb-2 mb-6">Detalhe Financeiro</h3>
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                            <div class="bg-blue-50 p-6 rounded-2xl border border-blue-100">
                                <label class="text-[10px] text-blue-500 font-bold uppercase block mb-1">Valor Total</label>
                                <p class="text-2xl font-black text-blue-700" id="out-preco"></p>
                            </div>
                            <div class="bg-slate-50 p-6 rounded-2xl border border-slate-100">
                                <label class="text-[10px] text-slate-400 font-bold uppercase block mb-1">Parcela Mensal</label>
                                <p class="text-2xl font-bold text-slate-700" id="out-parcela"></p>
                            </div>
                            <div class="bg-slate-50 p-6 rounded-2xl border border-slate-100">
                                <label class="text-[10px] text-slate-400 font-bold uppercase block mb-1">Entrada (1ª Parc.)</label>
                                <p class="text-2xl font-bold text-slate-700" id="out-entrada"></p>
                            </div>
                        </div>
                    </section>

                    <!-- Assinaturas -->
                    <section class="pt-16 pb-10">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-20">
                            <div class="text-center space-y-2">
                                <div class="border-t border-slate-300 pt-3">
                                    <p class="text-[10px] font-bold text-slate-400 uppercase tracking-widest">Assinatura do Consultor</p>
                                </div>
                            </div>
                            <div class="text-center space-y-2">
                                <div class="border-t border-slate-300 pt-3">
                                    <p class="text-[10px] font-bold text-slate-400 uppercase tracking-widest">Assinatura do Cliente</p>
                                </div>
                            </div>
                        </div>
                    </section>
                </div>

                <div class="bg-slate-900 p-4 text-center">
                    <p class="text-[9px] text-slate-500 font-bold uppercase tracking-[0.2em]">Documento gerado para fins informativos e de conferência.</p>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Iniciar Ícones
        lucide.createIcons();

        // Máscara CPF
        const cpfInput = document.getElementById('cpfInput');
        cpfInput.addEventListener('input', (e) => {
            let val = e.target.value.replace(/\D/g, '');
            if (val.length > 11) val = val.slice(0, 11);
            if (val.length > 9) val = val.replace(/(\d{3})(\d{3})(\d{3})(\d{1,2})/, '$1.$2.$3-$4');
            else if (val.length > 6) val = val.replace(/(\d{3})(\d{3})(\d{1,3})/, '$1.$2.$3');
            else if (val.length > 3) val = val.replace(/(\d{3})(\d{1,3})/, '$1.$2');
            e.target.value = val;
        });

        // Máscara Dinheiro
        document.querySelectorAll('.money').forEach(el => {
            el.addEventListener('input', (e) => {
                let value = e.target.value.replace(/\D/g, '');
                if (!value) { e.target.value = ''; return; }
                const formatted = new Intl.NumberFormat('pt-BR', {
                    style: 'currency',
                    currency: 'BRL'
                }).format(parseFloat(value) / 100);
                e.target.value = formatted;
            });
        });

        function toggleView(view) {
            if (view === 'form') {
                document.getElementById('view-form').classList.remove('hidden');
                document.getElementById('view-print').classList.add('hidden');
            } else {
                document.getElementById('view-form').classList.add('hidden');
                document.getElementById('view-print').classList.remove('hidden');
            }
            window.scrollTo(0, 0);
        }

        document.getElementById('propostaForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const fd = new FormData(this);
            
            // Atribuição de valores
            document.getElementById('out-nome').textContent = fd.get('nome');
            document.getElementById('out-cpf').textContent = fd.get('cpf');
            document.getElementById('out-categoria').textContent = fd.get('categoria');
            document.getElementById('out-preco').textContent = fd.get('preco');
            document.getElementById('out-parcela').textContent = fd.get('parcela');
            document.getElementById('out-entrada').textContent = fd.get('entrada');
            
            // Data
            const d = new Date();
            document.getElementById('out-data').textContent = `Emitido em ${d.toLocaleDateString('pt-BR')} às ${d.toLocaleTimeString('pt-BR', {hour: '2-digit', minute:'2-digit'})}`;

            // Ícones
            const iconMap = {
                'IMÓVEL': 'home',
                'REFORMA': 'hammer',
                'CONSTRUÇÃO': 'briefcase',
                'VEÍCULOS': 'truck',
                'RURAL': 'sprout',
                'CAPITAL DE GIRO': 'dollar-sign'
            };
            const iconName = iconMap[fd.get('categoria')] || 'file-text';
            const iconBox = document.getElementById('out-icon-box');
            iconBox.innerHTML = `<i data-lucide="${iconName}" class="w-8 h-8"></i>`;
            lucide.createIcons();

            toggleView('print');
        });
    </script>

</body>
</html>

