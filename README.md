<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MT PAINEL</title>

    <!-- Meta tags para otimização e SEO permanente -->
    <meta name="description" content="MT PAINEL - IPTV de alta performance e streamings.">
    <meta name="robots" content="index, follow">

    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Firebase App (SDK Core) -->
    <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-app-compat.js"></script>
    <!-- Firebase Firestore -->
    <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-firestore-compat.js"></script>

    <style>
        :root {
            --bg-color: #080808;
            --card-bg: #121212;
            --neon-red: #ff0033;
            --neon-glow: rgba(255, 0, 51, 0.4);
            --text-color: #ffffff;
            --gray-text: #b3b3b3;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background-color: var(--bg-color); color: var(--text-color); min-height: 100vh; padding-top: 90px; }

        /* HEADER */
        header {
            display: flex; justify-content: space-between; align-items: center;
            padding: 15px 30px; background: rgba(8, 8, 8, 0.98);
            border-bottom: 1px solid rgba(255, 0, 51, 0.3);
            position: fixed; top: 0; left: 0; width: 100%; z-index: 9999;
        }

        .brand-logo { display: flex; align-items: center; gap: 12px; }
        .neon-icon-badge {
            width: 40px; height: 40px; background: radial-gradient(circle, #2b0007 0%, #000 80%);
            border: 2px solid var(--neon-red); border-radius: 8px; display: flex;
            justify-content: center; align-items: center; color: var(--neon-red); font-size: 20px;
            box-shadow: 0 0 12px var(--neon-glow);
        }
        .logo-text { font-size: 22px; font-weight: 700; color: var(--neon-red); text-shadow: 0 0 10px var(--neon-glow); }
        .logo-text span { color: #fff; }

        .main-nav { display: flex; gap: 10px; }
        .nav-link {
            color: #fff; text-decoration: none; font-weight: 600; padding: 8px 14px;
            border-radius: 6px; transition: 0.3s; display: flex; align-items: center; gap: 8px;
            cursor: pointer; border: 1px solid transparent; font-size: 0.95rem; user-select: none;
        }
        .nav-link:hover, .nav-link.active {
            background-color: rgba(255, 0, 51, 0.15); color: var(--neon-red); border-color: var(--neon-red);
        }

        .hero {
            height: 35vh; display: flex; flex-direction: column; justify-content: center;
            align-items: center; text-align: center; padding: 0 20px;
            background: radial-gradient(circle at center, #260f13 0%, var(--bg-color) 70%);
        }
        .hero h1 { font-size: 2.5rem; margin-bottom: 10px; text-shadow: 0 0 20px rgba(255,0,51,0.3); }
        .hero p { font-size: 1.05rem; color: var(--gray-text); max-width: 600px; }

        .container { max-width: 1200px; margin: 0 auto; padding: 30px 20px; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }

        .section-title {
            font-size: 1.8rem; margin-bottom: 30px; border-left: 4px solid var(--neon-red);
            padding-left: 10px; text-shadow: 0 0 8px var(--neon-glow);
        }

        .plans-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 30px; }
        .plan-card {
            background-color: var(--card-bg); border-radius: 12px; padding: 25px;
            text-align: center; border: 1px solid #222; transition: all 0.3s ease;
            display: flex; flex-direction: column; justify-content: space-between;
        }
        .plan-card:hover { transform: translateY(-8px); border-color: var(--neon-red); box-shadow: 0 0 25px var(--neon-glow); }

        /* BANNERS METADE-METADE (EXCLUSIVO PARA COMBOS IPTV + STREAMING) */
        .split-banner {
            display: flex; height: 90px; border-radius: 8px; overflow: hidden;
            margin-bottom: 20px; border: 1px solid rgba(255,0,51,0.3);
        }
        .split-left {
            flex: 1; background: linear-gradient(135deg, #1f0007, #000); display: flex;
            flex-direction: column; justify-content: center; align-items: center; color: var(--neon-red);
            font-weight: 700; font-size: 0.85rem; border-right: 1px dashed rgba(255,0,51,0.3);
            text-shadow: 0 0 5px var(--neon-glow); text-align: center; padding: 5px; gap: 5px;
        }
        .split-right {
            flex: 1; display: flex; flex-direction: column; justify-content: center;
            align-items: center; font-weight: 600; font-size: 0.85rem; text-align: center; padding: 5px; gap: 5px;
        }

        /* CORES E GRADIENTES METADE-METADE PARA OS COMBOS */
        .split-style-globoplay { background: linear-gradient(135deg, #381a02, #000); color: #ffb700; }
        .split-style-disney { background: linear-gradient(135deg, #041238, #000); color: #6bfbff; }
        .split-style-hbo { background: linear-gradient(135deg, #1b0238, #000); color: #b185ff; }
        .split-style-netflix { background: linear-gradient(135deg, #380202, #000); color: #ff4d4d; }
        .split-style-prime { background: linear-gradient(135deg, #002b3d, #000); color: #00a8e1; }
        .split-style-sky { background: linear-gradient(135deg, #380005, #000); color: #ff2a00; }
        .split-style-claro { background: linear-gradient(135deg, #330000, #000); color: #ff0000; }

        /* BANNER DE ÍCONE ÚNICO (EXCLUSIVO PARA STREAMINGS INDIVIDUAIS) */
        .single-banner {
            height: 90px; border-radius: 8px; overflow: hidden;
            margin-bottom: 20px; border: 1px solid rgba(255,0,51,0.3);
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            font-weight: 700; font-size: 1rem; gap: 5px; padding: 10px;
        }
        .single-banner i { font-size: 28px; }

        /* CORES E GRADIENTES OFICIAIS PARA STREAMINGS INDIVIDUAIS */
        .style-globoplay { background: linear-gradient(135deg, #381a02, #000); color: #ffb700; border-color: rgba(255, 183, 0, 0.4); }
        .style-disney { background: linear-gradient(135deg, #041238, #000); color: #6bfbff; border-color: rgba(107, 251, 255, 0.4); }
        .style-hbo { background: linear-gradient(135deg, #1b0238, #000); color: #b185ff; border-color: rgba(177, 133, 255, 0.4); }
        .style-netflix { background: linear-gradient(135deg, #380202, #000); color: #ff4d4d; border-color: rgba(255, 77, 77, 0.4); }
        .style-prime { background: linear-gradient(135deg, #002b3d, #000); color: #00a8e1; border-color: rgba(0, 168, 225, 0.4); }
        .style-sky { background: linear-gradient(135deg, #380005, #000); color: #ff2a00; border-color: rgba(255, 42, 0, 0.4); }
        .style-claro { background: linear-gradient(135deg, #330000, #000); color: #ff0000; border-color: rgba(255, 0, 0, 0.4); }

        .plan-title { font-size: 1.2rem; margin-bottom: 10px; font-weight: 600; min-height: 50px; display: flex; align-items: center; justify-content: center; }
        .plan-price { font-size: 2rem; color: var(--neon-red); font-weight: 700; margin-bottom: 20px; text-shadow: 0 0 10px var(--neon-glow); }
        .plan-features { list-style: none; margin-bottom: 25px; text-align: left; color: var(--gray-text); }
        .plan-features li { margin-bottom: 8px; font-size: 0.9rem; }
        .plan-features i { color: var(--neon-red); margin-right: 8px; }

        .account-badge { font-size: 0.8rem; padding: 5px 10px; border-radius: 6px; margin-bottom: 15px; font-weight: 600; display: inline-flex; align-items: center; gap: 6px; justify-content: center; }
        .badge-shared { background: rgba(255, 170, 0, 0.15); border: 1px solid rgba(255, 170, 0, 0.4); color: #ffaa00; }
        .badge-private { background: rgba(0, 255, 136, 0.15); border: 1px solid rgba(0, 255, 136, 0.4); color: #00ff88; }

        .btn-neon {
            background-color: transparent; color: var(--neon-red); border: 2px solid var(--neon-red);
            padding: 12px 25px; border-radius: 6px; font-size: 1rem; font-weight: 600; cursor: pointer;
            width: 100%; transition: all 0.3s; text-shadow: 0 0 5px var(--neon-glow);
        }
        .btn-neon:hover { background-color: var(--neon-red); color: #fff; box-shadow: 0 0 20px var(--neon-red); }

        .btn-delete {
            background-color: #ff0033; color: white; border: none; padding: 6px 12px;
            border-radius: 4px; cursor: pointer; font-weight: 600; font-size: 0.85rem; transition: 0.2s;
        }
        .btn-delete:hover { background-color: #b30024; }

        /* EDITOR */
        .editor-container { background-color: var(--card-bg); border-radius: 12px; padding: 30px; border: 1px solid var(--neon-red); box-shadow: 0 0 25px var(--neon-glow); }
        .editor-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 30px; border-bottom: 1px solid #222; padding-bottom: 15px; }
        .editor-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 20px; }
        .editor-card { background-color: #080808; border: 1px solid #333; border-radius: 8px; padding: 20px; }
        .editor-card h4 { color: var(--neon-red); margin-bottom: 15px; display: flex; align-items: center; gap: 10px; font-size: 1.1rem; }

        .form-group { margin-bottom: 12px; text-align: left; }
        .form-group label { font-size: 0.85rem; color: var(--gray-text); display: block; margin-bottom: 5px; }
        .form-group input, .form-group select { width: 100%; padding: 8px 12px; background: #121212; border: 1px solid #333; border-radius: 4px; color: #fff; }

        /* MODAIS */
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.85); justify-content: center; align-items: center; z-index: 20000; }
        .modal-content { background-color: var(--card-bg); padding: 40px; border-radius: 12px; max-width: 450px; width: 90%; text-align: center; border: 2px solid var(--neon-red); box-shadow: 0 0 30px var(--neon-glow); position: relative; }
        .close-modal { position: absolute; top: 15px; right: 20px; font-size: 24px; color: var(--gray-text); cursor: pointer; }
        .pix-key-box { background: #000; padding: 15px; border-radius: 6px; margin: 20px 0; border: 1px dashed var(--neon-red); word-break: break-all; font-family: monospace; font-size: 1.1rem; color: var(--neon-red); }
        .btn-whatsapp { background-color: #25d366; color: #fff; border: none; padding: 14px 20px; border-radius: 6px; font-size: 1rem; font-weight: 600; cursor: pointer; width: 100%; display: flex; align-items: center; justify-content: center; gap: 10px; text-decoration: none; margin-top: 15px; }

        footer { text-align: center; padding: 30px; color: var(--gray-text); font-size: 0.9rem; border-top: 1px solid #1a1a1a; margin-top: 50px; }
    </style>
</head>
<body>

    <header>
        <div class="brand-logo">
            <div class="neon-icon-badge"><i class="fas fa-play"></i></div>
            <div class="logo-text">MT <span>PAINEL</span></div>
        </div>
        <nav class="main-nav">
            <div class="nav-link active" id="linkCombos" onclick="trocarAba('combosTab', 'linkCombos')"><i class="fas fa-cubes"></i> Combos IPTV</div>
            <!-- GATILHO OCULTO: 4 TOQUES AQUI ABREM O PAINEL DE EDITOR -->
            <div class="nav-link" id="linkStreamings" onclick="cliqueAbaStreamings()"><i class="fas fa-tv"></i> Streamings</div>
        </nav>
    </header>

    <section class="hero">
        <h1>O melhor do Entretenimento</h1>
        <p>IPTV de alta performance combinado com os seus streamings favoritos.</p>
    </section>

    <div class="container">
        
        <!-- ABA 1: COMBOS IPTV -->
        <main id="combosTab" class="tab-content active">
            <h2 class="section-title">Nossos Combos e Planos</h2>
            <div class="plans-grid" id="gridCombos"></div>
        </main>

        <!-- ABA 2: STREAMINGS INDIVIDUAIS -->
        <main id="streamingsTab" class="tab-content">
            <h2 class="section-title">Contas Individuais de Streaming</h2>
            <div class="plans-grid" id="gridStreamings"></div>
        </main>

        <!-- ABA 3: PAINEL DO EDITOR SEPARADO -->
        <main id="editorTab" class="tab-content">
            <div class="editor-container">
                <div class="editor-header">
                    <h2><i class="fas fa-tools" style="color: var(--neon-red);"></i> Painel do Editor</h2>
                    <button class="btn-neon" style="width: auto; padding: 6px 18px;" onclick="trocarAba('combosTab', 'linkCombos')">
                        <i class="fas fa-sign-out-alt"></i> Sair do Painel
                    </button>
                </div>

                <div class="editor-grid">
                    
                    <!-- 1. GERENCIAR COMBOS IPTV + STREAMING -->
                    <div class="editor-card">
                        <h4><i class="fas fa-cubes"></i> Gerenciar IPTV + Streaming</h4>
                        
                        <div class="form-group">
                            <label>Opção Pronta de Combo</label>
                            <select id="selectComboPronto" onchange="preencherComboPronto()">
                                <option value="">-- Selecione uma opção pronta --</option>
                                <option value="1 MÊS IPTV + GLOBOPLAY|R$ 40,00|IPTV (1 Mês)|GLOBOPLAY (1 Mês)|globoplay|compartilhada|1">1 Mês IPTV + Globoplay (R$ 40,00)</option>
                                <option value="2 MESES IPTV + DISNEY+|R$ 60,00|IPTV (2 Meses)|DISNEY+ (1 Mês)|disney|compartilhada|2">2 Meses IPTV + Disney+ (R$ 60,00)</option>
                                <option value="3 MESES IPTV + HBO MAX|R$ 85,00|IPTV (3 Meses)|HBO MAX (1 Mês)|hbo|compartilhada|3">3 Meses IPTV + HBO Max (R$ 85,00)</option>
                                <option value="1 MÊS IPTV + SKY+|R$ 50,00|IPTV (1 Mês)|SKY+ (1 Mês)|sky|compartilhada|1">1 Mês IPTV + SKY+ (R$ 50,00)</option>
                                <option value="1 MÊS IPTV + CLARO TV+|R$ 50,00|IPTV (1 Mês)|CLARO TV+ (1 Mês)|claro|compartilhada|1">1 Mês IPTV + Claro tv+ (R$ 50,00)</option>
                                <option value="6 MESES IPTV + GLOBOPLAY|R$ 155,00|IPTV (6 Meses)|GLOBOPLAY (1 Mês)|globoplay|compartilhada|6">6 Meses IPTV + Globoplay (R$ 155,00)</option>
                                <option value="12 MESES IPTV + NETFLIX|R$ 270,00|IPTV (12 Meses)|NETFLIX (1 Mês)|netflix|compartilhada|12">12 Meses IPTV + Netflix (R$ 270,00)</option>
                            </select>
                        </div>

                        <div class="form-group">
                            <label>1. Nome do Combo</label>
                            <input type="text" id="comboNome" placeholder="Ex: 1 MÊS IPTV + GLOBOPLAY">
                        </div>

                        <div class="form-group">
                            <label>2. Duração (em meses) para Ordenação</label>
                            <select id="comboOrdem">
                                <option value="1">1 Mês</option>
                                <option value="2">2 Meses</option>
                                <option value="3">3 Meses</option>
                                <option value="6">6 Meses</option>
                                <option value="12">12 Meses (1 Ano)</option>
                            </select>
                        </div>

                        <div class="form-group">
                            <label>3. Preço (R$)</label>
                            <input type="text" id="comboPreco" placeholder="Ex: R$ 40,00">
                        </div>

                        <div class="form-group">
                            <label>4. Texto Banner Esquerdo (IPTV)</label>
                            <input type="text" id="comboLeftText" placeholder="Ex: IPTV (1 Mês)">
                        </div>

                        <div class="form-group">
                            <label>5. Texto Banner Direito (Streaming)</label>
                            <input type="text" id="comboRightText" placeholder="Ex: GLOBOPLAY (1 Mês)">
                        </div>

                        <div class="form-group">
                            <label>6. Estilo / Cor do Streaming</label>
                            <select id="comboEstiloStreaming">
                                <option value="globoplay">Globoplay (Laranja)</option>
                                <option value="disney">Disney+ (Azul)</option>
                                <option value="hbo">Max / HBO (Roxo)</option>
                                <option value="netflix">Netflix (Vermelho)</option>
                                <option value="prime">Prime Video (Azul Claro)</option>
                                <option value="sky">SKY+ (Laranja/Vermelho)</option>
                                <option value="claro">Claro tv+ (Vermelho Claro)</option>
                            </select>
                        </div>

                        <div class="form-group">
                            <label>7. Tipo de Conta</label>
                            <select id="comboTipoConta">
                                <option value="compartilhada">Conta Compartilhada</option>
                                <option value="propria">Perfil Próprio / Exclusivo</option>
                            </select>
                        </div>

                        <button class="btn-neon" style="margin-top: 10px;" onclick="adicionarCombo()">Adicionar Combo</button>

                        <hr style="border-color: #222; margin: 20px 0;">
                        <label style="font-weight: 600; color: var(--gray-text); display: block; margin-bottom: 10px;">Excluir Combos Cadastrados:</label>
                        <div id="listaExclusaoCombos"></div>
                    </div>

                    <!-- 2. GERENCIAR STREAMINGS INDIVIDUAIS -->
                    <div class="editor-card">
                        <h4><i class="fas fa-tv"></i> Gerenciar Streamings Individuais</h4>
                        
                        <div class="form-group">
                            <label>Opção Pronta de Streaming</label>
                            <select id="selectPlanoPronto" onchange="preencherPlanoPronto()">
                                <option value="">-- Selecione uma opção pronta --</option>
                                <option value="Netflix Tela 4K|R$ 25,00|propria|netflix|NETFLIX - 1 Tela 4K|1">Netflix Tela 4K (R$ 25,00 - Própria)</option>
                                <option value="Disney+ Premium|R$ 22,00|compartilhada|disney|DISNEY+ - Premium|1">Disney+ Premium (R$ 22,00 - Compartilhada)</option>
                                <option value="Max (HBO)|R$ 20,00|compartilhada|hbo|MAX (HBO) - 1 Tela|1">Max (HBO) (R$ 20,00 - Compartilhada)</option>
                                <option value="Globoplay|R$ 18,00|compartilhada|globoplay|GLOBOPLAY - Sem Canais|1">Globoplay (R$ 18,00 - Compartilhada)</option>
                                <option value="Prime Video|R$ 15,00|propria|prime|PRIME VIDEO - 1 Tela|1">Prime Video (R$ 15,00 - Própria)</option>
                                <option value="SKY+ ao Vivo|R$ 30,00|compartilhada|sky|SKY+ - Canais ao Vivo|1">SKY+ ao Vivo (R$ 30,00 - Compartilhada)</option>
                                <option value="Claro tv+ HD|R$ 30,00|compartilhada|claro|CLARO TV+ - HD|1">Claro tv+ HD (R$ 30,00 - Compartilhada)</option>
                            </select>
                        </div>

                        <div class="form-group">
                            <label>1. Nome do Plano</label>
                            <input type="text" id="novoNome" placeholder="Ex: SKY+ ao Vivo">
                        </div>

                        <div class="form-group">
                            <label>2. Duração (em meses) para Ordenação</label>
                            <select id="novoOrdem">
                                <option value="1">1 Mês</option>
                                <option value="2">2 Meses</option>
                                <option value="3">3 Meses</option>
                                <option value="6">6 Meses</option>
                                <option value="12">12 Meses (1 Ano)</option>
                            </select>
                        </div>

                        <div class="form-group">
                            <label>3. Preço (R$)</label>
                            <input type="text" id="novoPreco" placeholder="Ex: R$ 30,00">
                        </div>

                        <div class="form-group">
                            <label>4. Texto do Banner</label>
                            <input type="text" id="novoTextoBanner" placeholder="Ex: SKY+ - Canais ao Vivo">
                        </div>

                        <div class="form-group">
                            <label>5. Estilo Visual e Cor Oficial</label>
                            <select id="novoEstilo">
                                <option value="netflix">Netflix (Vermelho)</option>
                                <option value="disney">Disney+ (Azul)</option>
                                <option value="hbo">Max / HBO (Roxo)</option>
                                <option value="globoplay">Globoplay (Laranja)</option>
                                <option value="prime">Prime Video (Azul Claro)</option>
                                <option value="sky">SKY+ (Laranja/Vermelho)</option>
                                <option value="claro">Claro tv+ (Vermelho Claro)</option>
                            </select>
                        </div>

                        <div class="form-group">
                            <label>6. Tipo de Conta</label>
                            <select id="novoTipoConta">
                                <option value="compartilhada">Conta Compartilhada</option>
                                <option value="propria">Perfil Próprio / Exclusivo</option>
                            </select>
                        </div>

                        <button class="btn-neon" style="margin-top: 10px;" onclick="adicionarPlanoStreaming()">Adicionar Streaming</button>

                        <hr style="border-color: #222; margin: 20px 0;">
                        <label style="font-weight: 600; color: var(--gray-text); display: block; margin-bottom: 10px;">Excluir Streamings Cadastrados:</label>
                        <div id="listaExclusaoStreamings"></div>
                    </div>

                    <!-- 3. CONFIGURAÇÕES DE SENHA -->
                    <div class="editor-card">
                        <h4><i class="fas fa-lock"></i> Alterar Senha do Editor</h4>
                        <div class="form-group">
                            <label>Nova Senha</label>
                            <input type="password" id="novaSenhaInput" placeholder="Digite a nova senha">
                        </div>
                        <button class="btn-neon" style="margin-top: 10px;" onclick="alterarSenhaEditor()">Atualizar Senha</button>
                    </div>

                </div>
            </div>
        </main>
    </div>

    <!-- MODAL DE SENHA DO EDITOR -->
    <div id="modalSenhaEditor" class="modal">
        <div class="modal-content">
            <span class="close-modal" onclick="fecharModalEditor()">&times;</span>
            <h2>Acesso Restrito</h2>
            <p style="color: var(--gray-text); margin-top: 10px;">Digite a senha do Editor:</p>
            <form id="formSenhaEditor" style="margin-top: 20px;">
                <div class="form-group">
                    <input type="password" id="campoSenhaEditor" placeholder="Senha do Editor" required style="text-align: center; font-size: 1.1rem;">
                </div>
                <button type="submit" class="btn-neon" style="margin-top: 10px;">Acessar Painel</button>
                <p id="msgErroEditor" style="color: var(--neon-red); margin-top: 10px; display: none;">Senha incorreta!</p>
            </form>
        </div>
    </div>

    <!-- MODAL PIX -->
    <div id="pixModal" class="modal">
        <div class="modal-content">
            <span class="close-modal" onclick="fecharModal()">&times;</span>
            <h2>Finalizar Pagamento</h2>
            <p style="color: var(--gray-text); margin-top: 10px;">Copie a chave Pix abaixo:</p>
            <div class="pix-key-box" id="pixKeyText">8a5e88d2-d62f-42d9-b08d-9943044fdce0</div>
            <button class="btn-neon" onclick="copiarChave()" style="margin-bottom: 15px; font-size: 0.9rem; padding: 8px;">
                <i class="fas fa-copy"></i> Copiar Chave Pix
            </button>
            <a id="whatsappBtn" href="#" target="_blank" rel="noopener noreferrer" class="btn-whatsapp">
                <i class="fab fa-whatsapp"></i> Enviar Comprovante no WhatsApp
            </a>
        </div>
    </div>

    <footer>
        <p>&copy; 2026 MT PAINEL. Todos os direitos reservados.</p>
    </footer>

    <script>
        // CONFIGURAÇÃO DO FIREBASE PARA mtpainel2 (COLOQUE SUAS CHAVES AQUI)
        const firebaseConfig = {
            apiKey: "SUA_API_KEY",
            authDomain: "mtpainel2.firebaseapp.com",
            projectId: "mtpainel2",
            storageBucket: "mtpainel2.appspot.com",
            messagingSenderId: "SEU_MESSAGING_SENDER_ID",
            appId: "SEU_APP_ID"
        };

        // Inicializar Firebase
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();

        const CHAVE_PIX = "8a5e88d2-d62f-42d9-b08d-9943044fdce0"; 
        const NUMERO_WHATSAPP = "5581995687814"; 
        let senhaEditorAtual = "0304"; 

        let listaCombos = [];
        let listaStreamings = [];

        // MAPA DE ÍCONES
        const mapaIcones = {
            'netflix': 'fab fa-netflix',
            'disney': 'fas fa-dragon',
            'hbo': 'fas fa-tv',
            'globoplay': 'fas fa-film',
            'prime': 'fas fa-play-circle',
            'sky': 'fas fa-satellite-dish',
            'claro': 'fas fa-broadcast-tower'
        };

        // CARREGAR DADOS DO FIREBASE EM TEMPO REAL E ORDENADOS POR MESES (asc)
        function carregarDadosDoFirebase() {
            db.collection("combos").orderBy("ordem", "asc").onSnapshot((snapshot) => {
                listaCombos = [];
                snapshot.forEach((doc) => {
                    listaCombos.push({ id: doc.id, ...doc.data() });
                });
                renderizarCombos();
            }, (error) => {
                db.collection("combos").onSnapshot((snapshot) => {
                    listaCombos = [];
                    snapshot.forEach((doc) => {
                        listaCombos.push({ id: doc.id, ...doc.data() });
                    });
                    listaCombos.sort((a, b) => (Number(a.ordem) || 0) - (Number(b.ordem) || 0));
                    renderizarCombos();
                });
            });

            db.collection("streamings").orderBy("ordem", "asc").onSnapshot((snapshot) => {
                listaStreamings = [];
                snapshot.forEach((doc) => {
                    listaStreamings.push({ id: doc.id, ...doc.data() });
                });
                renderizarStreamings();
            }, (error) => {
                db.collection("streamings").onSnapshot((snapshot) => {
                    listaStreamings = [];
                    snapshot.forEach((doc) => {
                        listaStreamings.push({ id: doc.id, ...doc.data() });
                    });
                    listaStreamings.sort((a, b) => (Number(a.ordem) || 0) - (Number(b.ordem) || 0));
                    renderizarStreamings();
                });
            });
        }

        // RENDERIZAR COMBOS IPTV
        function renderizarCombos() {
            const gridCombos = document.getElementById('gridCombos');
            const listaExclusao = document.getElementById('listaExclusaoCombos');

            gridCombos.innerHTML = '';
            listaExclusao.innerHTML = '';

            listaCombos.forEach(combo => {
                const icone = mapaIcones[combo.estilo] || 'fas fa-film';
                const badgeHtml = combo.conta === 'propria' 
                    ? `<div class="account-badge badge-private"><i class="fas fa-user-check"></i> Perfil Próprio</div>`
                    : `<div class="account-badge badge-shared"><i class="fas fa-users"></i> Conta Streaming Compartilhada</div>`;

                gridCombos.innerHTML += `
                    <div class="plan-card">
                        <div>
                            <div class="split-banner">
                                <div class="split-left"><i class="fas fa-tv"></i> ${combo.leftText}</div>
                                <div class="split-right split-style-${combo.estilo}"><i class="${icone}"></i> ${combo.rightText}</div>
                            </div>
                            <h3 class="plan-title">${combo.nome}</h3>
                            <div class="plan-price">${combo.preco}</div>
                            ${badgeHtml}
                            <ul class="plan-features">
                                <li><i class="fas fa-check"></i> IPTV: Acesso Completo</li>
                                <li><i class="fas fa-check"></i> Streaming Incluído</li>
                                <li><i class="fas fa-check"></i> Qualidade HD e 4K</li>
                                <li><i class="fas fa-check"></i> Suporte Dedicado</li>
                            </ul>
                        </div>
                        <button class="btn-neon" onclick="abrirModal('${combo.nome}', '${combo.preco}')">Assinar Agora</button>
                    </div>
                `;

                listaExclusao.innerHTML += `
                    <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:8px; background:#121212; padding:8px 12px; border-radius:4px; border:1px solid #222;">
                        <span style="font-size:0.85rem;">${combo.nome} (${combo.preco})</span>
                        <button class="btn-delete" onclick="excluirCombo('${combo.id}')"><i class="fas fa-trash"></i> Excluir</button>
                    </div>
                `;
            });
        }

        // RENDERIZAR STREAMINGS
        function renderizarStreamings() {
            const gridStreamings = document.getElementById('gridStreamings');
            const listaExclusao = document.getElementById('listaExclusaoStreamings');

            gridStreamings.innerHTML = '';
            listaExclusao.innerHTML = '';

            listaStreamings.forEach(plano => {
                const icone = mapaIcones[plano.estilo] || 'fas fa-tv';
                const badgeHtml = plano.conta === 'propria' 
                    ? `<div class="account-badge badge-private"><i class="fas fa-user-check"></i> Perfil Próprio</div>`
                    : `<div class="account-badge badge-shared"><i class="fas fa-users"></i> Conta Compartilhada</div>`;

                gridStreamings.innerHTML += `
                    <div class="plan-card">
                        <div>
                            <div class="single-banner style-${plano.estilo}">
                                <i class="${icone}"></i>
                                <span>${plano.textoBanner}</span>
                            </div>
                            <h3 class="plan-title">${plano.nome}</h3>
                            <div class="plan-price">${plano.preco}</div>
                            ${badgeHtml}
                            <ul class="plan-features">
                                <li><i class="fas fa-check"></i> Qualidade Ultra HD / 4K</li>
                                <li><i class="fas fa-check"></i> Validade: 30 Dias</li>
                                <li><i class="fas fa-check"></i> Suporte e Garantia</li>
                            </ul>
                        </div>
                        <button class="btn-neon" onclick="abrirModal('${plano.nome}', '${plano.preco}')">Assinar Agora</button>
                    </div>
                `;

                listaExclusao.innerHTML += `
                    <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:8px; background:#121212; padding:8px 12px; border-radius:4px; border:1px solid #222;">
                        <span style="font-size:0.85rem;">${plano.nome} (${plano.preco})</span>
                        <button class="btn-delete" onclick="excluirPlano('${plano.id}')"><i class="fas fa-trash"></i> Excluir</button>
                    </div>
                `;
            });
        }

        // PREENCHIMENTO RÁPIDO - COMBOS
        function preencherComboPronto() {
            const val = document.getElementById('selectComboPronto').value;
            if (val) {
                const [nome, preco, leftText, rightText, estilo, conta, ordem] = val.split('|');
                document.getElementById('comboNome').value = nome;
                document.getElementById('comboPreco').value = preco;
                document.getElementById('comboLeftText').value = leftText;
                document.getElementById('comboRightText').value = rightText;
                document.getElementById('comboEstiloStreaming').value = estilo;
                document.getElementById('comboTipoConta').value = conta;
                document.getElementById('comboOrdem').value = ordem || 1;
            }
        }

        // PREENCHIMENTO RÁPIDO - STREAMINGS
        function preencherPlanoPronto() {
            const val = document.getElementById('selectPlanoPronto').value;
            if (val) {
                const [nome, preco, conta, estilo, textoBanner, ordem] = val.split('|');
                document.getElementById('novoNome').value = nome;
                document.getElementById('novoPreco').value = preco;
                document.getElementById('novoTipoConta').value = conta;
                document.getElementById('novoEstilo').value = estilo;
                document.getElementById('novoTextoBanner').value = textoBanner;
                document.getElementById('novoOrdem').value = ordem || 1;
            }
        }

        // ADICIONAR COMBO
        function adicionarCombo() {
            const nome = document.getElementById('comboNome').value;
            const preco = document.getElementById('comboPreco').value;
            const leftText = document.getElementById('comboLeftText').value;
            const rightText = document.getElementById('comboRightText').value;
            const estilo = document.getElementById('comboEstiloStreaming').value;
            const conta = document.getElementById('comboTipoConta').value;
            const ordem = Number(document.getElementById('comboOrdem').value) || 1;

            if (!nome || !preco || !leftText || !rightText) {
                alert('Por favor, preencha todos os campos do Combo!');
                return;
            }

            const novoCombo = { nome, preco, leftText, rightText, estilo, conta, ordem };
            
            db.collection("combos").add(novoCombo).then(() => {
                document.getElementById('comboNome').value = '';
                document.getElementById('comboPreco').value = '';
                document.getElementById('comboLeftText').value = '';
                document.getElementById('comboRightText').value = '';
                document.getElementById('selectComboPronto').value = '';
                alert('Combo cadastrado com sucesso!');
            }).catch((error) => {
                alert("Erro ao salvar combo: " + error.message);
            });
        }

        // ADICIONAR STREAMING
        function adicionarPlanoStreaming() {
            const nome = document.getElementById('novoNome').value;
            const preco = document.getElementById('novoPreco').value;
            const conta = document.getElementById('novoTipoConta').value;
            const estilo = document.getElementById('novoEstilo').value;
            const textoBanner = document.getElementById('novoTextoBanner').value || nome;
            const ordem = Number(document.getElementById('novoOrdem').value) || 1;

            if (!nome || !preco) {
                alert('Por favor, preencha o nome e o preço!');
                return;
            }

            const novoPlano = { nome, preco, conta, estilo, textoBanner, ordem };

            db.collection("streamings").add(novoPlano).then(() => {
                document.getElementById('novoNome').value = '';
                document.getElementById('novoPreco').value = '';
                document.getElementById('novoTextoBanner').value = '';
                document.getElementById('selectPlanoPronto').value = '';
                alert('Streaming adicionado com sucesso!');
            }).catch((error) => {
                alert("Erro ao salvar streaming: " + error.message);
            });
        }

        function excluirPlano(id) {
            if (confirm('Tem certeza que deseja excluir este streaming?')) {
                db.collection("streamings").doc(id).delete().then(() => alert('Streaming excluído com sucesso!'));
            }
        }

        function excluirCombo(id) {
            if (confirm('Tem certeza que deseja excluir este combo?')) {
                db.collection("combos").doc(id).delete().then(() => alert('Combo excluído com sucesso!'));
            }
        }

        // GATILHO DE 4 CLIQUES NA ABA STREAMINGS
        let contadorCliquesStreamings = 0;
        let timerCliquesStreamings = null;

        function cliqueAbaStreamings() {
            trocarAba('streamingsTab', 'linkStreamings');
            contadorCliquesStreamings++;
            clearTimeout(timerCliquesStreamings);

            if (contadorCliquesStreamings === 4) {
                contadorCliquesStreamings = 0;
                abrirModalEditor();
            } else {
                timerCliquesStreamings = setTimeout(() => { contadorCliquesStreamings = 0; }, 1500);
            }
        }

        function trocarAba(tabId, linkId) {
            document.querySelectorAll('.tab-content').forEach(tab => tab.classList.remove('active'));
            document.querySelectorAll('.nav-link').forEach(link => link.classList.remove('active'));
            document.getElementById(tabId).classList.add('active');
            if (linkId) document.getElementById(linkId).classList.add('active');
        }

        function abrirModalEditor() {
            document.getElementById("modalSenhaEditor").style.display = "flex";
            document.getElementById("msgErroEditor").style.display = "none";
            document.getElementById("campoSenhaEditor").value = "";
        }

        function fecharModalEditor() { document.getElementById("modalSenhaEditor").style.display = "none"; }

        document.getElementById("formSenhaEditor").addEventListener("submit", function(e) {
            e.preventDefault();
            if (document.getElementById("campoSenhaEditor").value === senhaEditorAtual) {
                fecharModalEditor();
                trocarAba('editorTab', null);
            } else {
                document.getElementById("msgErroEditor").style.display = "block";
            }
        });

        function alterarSenhaEditor() {
            const novaSenha = document.getElementById("novaSenhaInput").value;
            if (novaSenha.trim() !== "") {
                senhaEditorAtual = novaSenha;
                alert("Senha do editor alterada com sucesso!");
                document.getElementById("novaSenhaInput").value = "";
            }
        }

        function abrirModal(nomePlano, precoPlano) {
            const modal = document.getElementById("pixModal");
            const whatsappBtn = document.getElementById("whatsappBtn");
            const mensagem = encodeURIComponent(`Olá, comprei o plano (${nomePlano} - ${precoPlano}). Segue o comprovante!`);
            whatsappBtn.href = `https://wa.me/${NUMERO_WHATSAPP}?text=${mensagem}`;
            modal.style.display = "flex";
        }

        function fecharModal() { document.getElementById("pixModal").style.display = "none"; }
        function copiarChave() {
            navigator.clipboard.writeText(CHAVE_PIX).then(() => alert("Chave Pix copiada com sucesso!"));
        }

        window.onclick = function(e) {
            if (e.target == document.getElementById("pixModal")) fecharModal();
            if (e.target == document.getElementById("modalSenhaEditor")) fecharModalEditor();
        }

        carregarDadosDoFirebase();
    </script>
</body>
</html>
