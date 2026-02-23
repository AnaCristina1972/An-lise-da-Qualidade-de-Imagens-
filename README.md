# Implementação, Avaliação e Teste de Métricas Full Reference de Qualidade de Imagens

> Trabalho acadêmico — Disciplina: Análise da Qualidade de Imagens  
> Impact-Lab 2025/2 · Instituto de Computação (IComp) · UFAM

**Autores:** Ana Cristina da Silva Vieira · Franklin Xavier · Nayra Vieira · Charlene Queiroz

---

## 📋 Descrição

Implementação e avaliação comparativa de **13 métricas Full Reference (FR-IQA)** sobre 7 imagens de características fotométricas distintas, com **105 experimentos controlados** cobrindo 5 tipos de distorção. O projeto inclui também um protocolo de avaliação subjetiva (DSCQS) com 10 observadores humanos e análise do resíduo entre modos de captura de smartphone.

---

## 📁 Estrutura do Repositório

```
.
├── projeto_qualidade_imagens.ipynb   # Pipeline completo FR-IQA (Python)
├── main.tex                          # Artigo científico (LaTeX / SBC)
├── sbc-template.bib                  # Referências bibliográficas
├── sbc-template.sty                  # Estilo SBC
├── sbc.bst                           # BibTeX style
│
├── imagens/
│   ├── Brickhouse.bmp                # Base LIVE Immersive
│   ├── Castle.bmp
│   ├── Livingroom.bmp
│   ├── Brightroom.bmp
│   ├── Darkroom.bmp
│   ├── foto Automatica.jpeg          # Xiaomi POCO X5 Pro 5G — ISO 1928
│   └── foto Manual.jpeg              # Xiaomi POCO X5 Pro 5G — ISO 2856
│
├── figuras/
│   ├── grade_imagens.png
│   ├── fluxograma_pipeline_FRIQA.png
│   ├── viz_01_gauss25_Brickhouse.png
│   ├── viz_06_blur15_Brickhouse.png
│   ├── viz_07_jpeg10_Brickhouse.png
│   ├── 01_psnr_ssim_ruido_Brickhouse.png
│   ├── 02_metricas_desfoque_Brickhouse.png
│   ├── 03_metricas_jpeg_Brickhouse.png
│   ├── 04_heatmap_metricas_Brickhouse.png
│   └── 05_correlacao_metricas_Brickhouse.png
│
└── resultado_*/                      # 7 pastas — uma por imagem
    ├── graficos/                     # 5 gráficos analíticos por imagem
    │   ├── 01_psnr_ssim_ruido_*.png
    │   ├── 02_metricas_desfoque_*.png
    │   ├── 03_metricas_jpeg_*.png
    │   ├── 04_heatmap_metricas_*.png
    │   └── 05_correlacao_metricas_*.png
    ├── visualizacoes/                # 10 trios (original|distorcida|mapa) por imagem
    │   ├── viz_01_gauss25_*.png
    │   ├── viz_02_gauss50_*.png
    │   ├── viz_03_sp005_*.png
    │   ├── viz_04_sp010_*.png
    │   ├── viz_05_blur7_*.png
    │   ├── viz_06_blur15_*.png
    │   ├── viz_07_jpeg10_*.png
    │   ├── viz_08_jpeg50_*.png
    │   ├── viz_09_brilho_escuro_*.png
    │   └── viz_10_brilho_claro_*.png
    └── dados/                        # CSVs + análise crítica por imagem
        ├── resultados_completos_*.csv
        ├── resultado_Ruído_Gaussiano_*.csv
        ├── resultado_Sal-e-Pimenta_*.csv
        ├── resultado_Desfoque_*.csv
        ├── resultado_JPEG_*.csv
        ├── resultado_Brilho_Contraste_*.csv
        ├── imagem_original_*.png
        └── analise_critica_*.txt
```

---

## 🔬 Métricas Implementadas

### Clássicas (paradigma MSE)
| Métrica | Descrição |
|---|---|
| MSE | Mean Squared Error |
| RMSE | Root Mean Squared Error |
| PSNR | Peak Signal-to-Noise Ratio |
| MAE | Mean Absolute Error |
| NAE | Normalized Absolute Error |
| NCC | Normalized Cross-Correlation |
| Fidelity | Preservação de energia do sinal |
| Accuracy | 1 − NAE |

### Perceptuais
| Métrica | Descrição |
|---|---|
| SSIM | Structural Similarity Index |
| MS-SSIM | Multi-Scale SSIM |
| UIQ | Universal Image Quality Index |
| FSIM | Feature Similarity Index (Phase Congruency) |
| VIF | Visual Information Fidelity |

---

## 🖼️ Base de Dados

### LIVE Immersive Image Database
5 imagens panorâmicas 360° (BMP, lossless), redimensionadas para 512×512 px:

| Imagem | Brilho médio | Textura (Laplaciano) |
|---|---|---|
| Brickhouse | 61,3 | 174 |
| Castle | 178,5 | 1239 |
| Livingroom | 198,7 | 710 |
| Brightroom | 109,2 | 280 |
| Darkroom | 53,7 | 91 |

### Fotografias de Smartphone
Par de fotos capturadas com **Xiaomi POCO X5 Pro 5G** na mesma cena (21/02/2026, 02:28):

| Parâmetro | Automático | Manual |
|---|---|---|
| ISO | **1928** | **2856** |
| Abertura | f/1,89 | f/1,89 |
| Exposição | 1/10 s | 1/10 s |
| Tamanho | 1,98 MB | 2,29 MB |

---

## ⚗️ Distorções Aplicadas

| Grupo | Distorção | Parâmetros |
|---|---|---|
| Ruído Gaussiano | N(0, σ²) | σ ∈ {10, 25, 50} |
| Ruído Sal-e-Pimenta | Impulso bilateral | d ∈ {0,01, 0,05, 0,10} |
| Desfoque Gaussiano | Filtro gaussiano | k ∈ {3×3, 7×7, 15×15} |
| Compressão JPEG | Codec JPEG | Q ∈ {10, 30, 50, 90} |
| Brilho/Contraste | g = αf + β | (α,β) ∈ {(0,5,−50), (1,5,+50)} |

---

## 🚀 Como Usar

### Requisitos

```bash
pip install numpy opencv-python matplotlib scipy pillow
```

### Executar o Pipeline

1. Abra `projeto_qualidade_imagens.ipynb` no **Google Colab** ou Jupyter
2. Na **Célula 9**, configure o caminho da imagem:
   ```python
   IMAGE_PATH = 'imagens/Brickhouse.bmp'
   ```
3. Execute todas as células em ordem

### Saídas Geradas

Para cada imagem processada, o sistema cria automaticamente `resultado_NomeDaImagem/` com:

```
resultado_NomeDaImagem/
├── graficos/          # 5 gráficos analíticos (PNG)
├── visualizacoes/     # 10 visualizações: original | distorcida | mapa de diferença
└── dados/             # 6 CSVs por grupo + CSV completo + análise crítica (.txt)
```

**Total por imagem:** 15 experimentos · 13 métricas · 10 visualizações · 5 gráficos · 6 CSVs
**Total geral (7 imagens):** 105 experimentos · 70 visualizações · 35 gráficos · 42 arquivos CSV

---

## 📊 Principais Resultados

### Ranking de Sensibilidade das Métricas

| Posição | Métrica | Δ médio | Consistência |
|---|---|---|---|
| 1º | MS-SSIM | 0,856 | Alta |
| 2º | SSIM | 0,882 | Alta |
| 3º | VIF | 0,802 | Média |
| 4º | UIQ | 0,737 | Média |
| 5º | FSIM | 0,541 | Muito alta |
| 6º | PSNR | 32,85 dB | Baixa |
| 7º | NCC | 0,233 | Baixa |

### Resíduo entre Modos de Captura (Smartphone)

| Métrica | Valor |
|---|---|
| Média do resíduo R̄ | +28,17 pixels |
| Desvio padrão σY | 61,85 pixels |
| Variância σY² | 3825,23 pixels² |
| SNR | **2,96 dB** |

### Avaliação Subjetiva DSCQS (N=5 por grupo)

| Grupo | MOS Manual (A) | MOS Automático (B) | Δ |
|---|---|---|---|
| Notebook (calibrado) | 3,60 | 3,40 | −0,20 |
| Smartphone (livre) | 3,60 | **4,20** | +0,60 |

---

## 📄 Artigo

O artigo completo em LaTeX está em `main.tex`, formatado no template **SBC (Sociedade Brasileira de Computação)**.

Para compilar no Overleaf:
1. Faça upload de todos os arquivos `.tex`, `.bib`, `.sty`, `.bst` e das imagens
2. Compile com **pdfLaTeX**

---

## 🔗 Material Suplementar

Todo o material gerado (código, imagens, CSVs e gráficos) está disponível em:  
📁 [Google Drive — Impact-Lab IQA 2025/2](https://drive.google.com/drive/folders/1cP1kKH9-4T4tDuJGaaJBrO8s5oKTImwa)

---

## 📚 Referências Principais

- Wang et al. (2004). *Image quality assessment: from error visibility to structural similarity.* IEEE TIP.
- Wang & Bovik (2009). *Mean squared error: Love it or leave it?* IEEE SPM.
- Wang et al. (2003). *Multiscale structural similarity for image quality assessment.* Asilomar.
- Sheikh & Bovik (2006). *Image information and visual quality.* IEEE TIP.
- Zhang et al. (2011). *FSIM: A feature similarity index for image quality assessment.* IEEE TIP.

---

## 📝 Licença

Trabalho acadêmico — Impact-Lab 2025/2 · IComp · UFAM. Uso restrito a fins educacionais.
