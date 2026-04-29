import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.patches as mpatches
import matplotlib.gridspec as gridspec

df = pd.read_csv('credit_card_fraud_10k.csv')

# ---- RƏNGLƏR ----
DARK_BLUE  = '#1F3864'
MED_BLUE   = '#2F75B6'
LIGHT_BLUE = '#BDD7EE'
RED        = '#C00000'
GRAY       = '#F2F2F2'
TEXT       = '#1F1F1F'
MUTED      = '#666666'

# ---- DATA HAZIRLIĞI ----
fraud_hour = df.groupby('transaction_hour')['is_fraud'].agg(['sum','count']).reset_index()
fraud_hour.columns = ['hour', 'fraud', 'total']
fraud_hour['rate'] = fraud_hour['fraud'] / fraud_hour['total'] * 100

fraud_cat = df.groupby('merchant_category')['is_fraud'].agg(['sum','count']).reset_index()
fraud_cat.columns = ['cat', 'fraud', 'total']
fraud_cat['rate'] = fraud_cat['fraud'] / fraud_cat['total'] * 100
fraud_cat = fraud_cat.sort_values('fraud', ascending=True)

foreign    = df.groupby('foreign_transaction')['is_fraud'].mean() * 100
amt_fraud  = df[df['is_fraud']==1]['amount'].mean()
amt_normal = df[df['is_fraud']==0]['amount'].mean()

# ---- FİGURE QURULUMU ----
fig = plt.figure(figsize=(16, 20), facecolor='white')
gs  = gridspec.GridSpec(4, 2, figure=fig,
        hspace=0.55, wspace=0.35,
        top=0.93, bottom=0.04, left=0.07, right=0.97)

fig.text(0.5, 0.97, 'Kredit Kartı Fraud Analizi',
    ha='center', va='top', fontsize=22, fontweight='bold', color=DARK_BLUE)
fig.text(0.5, 0.945, '10,000 tranzaksiya əsasında — Python ilə hazırlanmışdır',
    ha='center', va='top', fontsize=11, color=MUTED)

# ---- KPI KARTLARI ----
ax_kpi = fig.add_subplot(gs[0, :])
ax_kpi.set_xlim(0, 1)
ax_kpi.set_ylim(0, 1)
ax_kpi.axis('off')

kpis = [
    (0.08, "10,000", "Ümumi Tranzaksiya", MED_BLUE),
    (0.30, "151",    "Fraud Sayı",         RED),
    (0.52, "1.5%",   "Fraud Nisbəti",      DARK_BLUE),
    (0.74, "$216",   "Orta Fraud Məbləği", MED_BLUE),
]
for x, val, label, color in kpis:
    fancy = mpatches.FancyBboxPatch((x, 0.08), 0.18, 0.82,
        boxstyle="round,pad=0.02", linewidth=0,
        facecolor=color, transform=ax_kpi.transAxes)
    ax_kpi.add_patch(fancy)
    ax_kpi.text(x + 0.09, 0.62, val,
        ha='center', va='center', fontsize=22, fontweight='bold',
        color='white', transform=ax_kpi.transAxes)
    ax_kpi.text(x + 0.09, 0.25, label,
        ha='center', va='center', fontsize=9.5, color='#CCDDEE',
        transform=ax_kpi.transAxes)
ax_kpi.set_title('Əsas Göstəricilər', fontsize=12, fontweight='bold',
    color=DARK_BLUE, pad=8, loc='left')

# ---- CHART 1: SAATA GÖRƏ FRAUD ----
ax1 = fig.add_subplot(gs[1, :])
hours      = fraud_hour['hour'].values
fraud_vals = fraud_hour['fraud'].values
colors_h   = [RED if h < 4 else LIGHT_BLUE for h in hours]

bars = ax1.bar(hours, fraud_vals, color=colors_h, width=0.7, zorder=3)
for bar, val in zip(bars, fraud_vals):
    if val > 10:
        ax1.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 0.5,
            str(val), ha='center', va='bottom', fontsize=8,
            fontweight='bold', color=RED)

ax1.set_facecolor(GRAY)
ax1.grid(axis='y', color='white', linewidth=1.2, zorder=2)
ax1.set_xticks(hours)
ax1.set_xticklabels([f'{h:02d}:00' for h in hours], rotation=45, fontsize=8)
ax1.set_xlabel('Saat', fontsize=10, color=MUTED)
ax1.set_ylabel('Fraud Sayı', fontsize=10, color=MUTED)
ax1.set_title('Saata Görə Fraud Sayı', fontsize=12, fontweight='bold', color=DARK_BLUE, pad=10)
ax1.spines[['top','right','left','bottom']].set_visible(False)
ax1.tick_params(left=False)

legend_p = [
    mpatches.Patch(color=RED,        label='Gecə (00:00–03:59) — Yüksək Risk'),
    mpatches.Patch(color=LIGHT_BLUE, label='Gündüz — Aşağı Risk'),
]
ax1.legend(handles=legend_p, loc='upper right', fontsize=9, framealpha=0.9, edgecolor='none')
ax1.annotate('81% fraud\ngecə baş verir', xy=(1.5, 34),
    xytext=(6, 30), fontsize=9, color=RED, fontweight='bold',
    arrowprops=dict(arrowstyle='->', color=RED, lw=1.5))

# ---- CHART 2: KATEQORİYA ----
ax2 = fig.add_subplot(gs[2, 0])
cats     = fraud_cat['cat'].values
fraud_c  = fraud_cat['fraud'].values
colors_c = [RED if v == max(fraud_c) else MED_BLUE for v in fraud_c]

ax2.barh(cats, fraud_c, color=colors_c, height=0.6, zorder=3)
for i, v in enumerate(fraud_c):
    ax2.text(v + 0.3, i, str(v), va='center', fontsize=9,
        fontweight='bold', color=RED if v == max(fraud_c) else TEXT)
ax2.set_facecolor(GRAY)
ax2.grid(axis='x', color='white', linewidth=1.2, zorder=2)
ax2.set_xlabel('Fraud Sayı', fontsize=10, color=MUTED)
ax2.set_title('Kateqoriyaya Görə Fraud', fontsize=12, fontweight='bold', color=DARK_BLUE, pad=10)
ax2.spines[['top','right','left','bottom']].set_visible(False)
ax2.tick_params(bottom=False)

# ---- CHART 3: XARİCİ vs YERLİ ----
ax3 = fig.add_subplot(gs[2, 1])
vals3 = [foreign[0], foreign[1]]
bars3 = ax3.bar(['Yerli\nTranzaksiya', 'Xarici\nTranzaksiya'],
    vals3, color=[MED_BLUE, RED], width=0.5, zorder=3)
for bar, val in zip(bars3, vals3):
    ax3.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 0.1,
        f'{val:.2f}%', ha='center', va='bottom', fontsize=12, fontweight='bold',
        color=RED if val > 5 else MED_BLUE)
ax3.set_facecolor(GRAY)
ax3.grid(axis='y', color='white', linewidth=1.2, zorder=2)
ax3.set_ylabel('Fraud Nisbəti (%)', fontsize=10, color=MUTED)
ax3.set_title('Xarici vs Yerli Fraud Riski', fontsize=12, fontweight='bold', color=DARK_BLUE, pad=10)
ax3.spines[['top','right','left','bottom']].set_visible(False)
ax3.tick_params(bottom=False, left=False)
ax3.text(0.5, 0.5, '11x\nyüksək risk', ha='center', va='center',
    fontsize=13, fontweight='bold', color=RED, transform=ax3.transAxes,
    bbox=dict(boxstyle='round,pad=0.4', facecolor='white', edgecolor=RED, linewidth=1.5, alpha=0.9))

# ---- CHART 4: MƏBLƏĞ MÜQAYİSƏSİ ----
ax4 = fig.add_subplot(gs[3, 0])
vals4 = [amt_normal, amt_fraud]
bars4 = ax4.bar(['Normal\nTranzaksiya', 'Fraud\nTranzaksiya'],
    vals4, color=[MED_BLUE, RED], width=0.5, zorder=3)
for bar, val in zip(bars4, vals4):
    ax4.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 1,
        f'${val:.0f}', ha='center', va='bottom', fontsize=13, fontweight='bold',
        color=RED if val > 200 else MED_BLUE)
ax4.set_facecolor(GRAY)
ax4.grid(axis='y', color='white', linewidth=1.2, zorder=2)
ax4.set_ylabel('Orta Məbləğ ($)', fontsize=10, color=MUTED)
ax4.set_title('Orta Tranzaksiya Məbləği', fontsize=12, fontweight='bold', color=DARK_BLUE, pad=10)
ax4.spines[['top','right','left','bottom']].set_visible(False)
ax4.tick_params(bottom=False, left=False)
ax4.set_ylim(0, 270)
diff_pct = (amt_fraud - amt_normal) / amt_normal * 100
ax4.text(0.5, 0.88, f'Fraud {diff_pct:.0f}% yüksəkdir',
    ha='center', transform=ax4.transAxes, fontsize=9, color=RED, fontweight='bold',
    bbox=dict(boxstyle='round,pad=0.3', facecolor='white', edgecolor=RED, linewidth=1, alpha=0.9))

# ---- CHART 5: PIE CHART ----
ax5 = fig.add_subplot(gs[3, 1])
wedges, texts, autotexts = ax5.pie(
    [9849, 151], labels=['Normal (98.5%)', 'Fraud (1.5%)'],
    colors=[LIGHT_BLUE, RED], explode=(0, 0.06), autopct='%1.1f%%',
    startangle=90, pctdistance=0.75,
    textprops={'fontsize': 9, 'color': TEXT},
    wedgeprops={'linewidth': 2, 'edgecolor': 'white'})
for at in autotexts:
    at.set_fontsize(9)
    at.set_fontweight('bold')
ax5.set_title('Fraud vs Normal Tranzaksiya', fontsize=12,
    fontweight='bold', color=DARK_BLUE, pad=10)

plt.savefig('fraud_analiz.png', dpi=150, bbox_inches='tight', facecolor='white')
plt.show()
print("Dashboard hazırdır!")
