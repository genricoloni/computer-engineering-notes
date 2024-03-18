# Cheatsheet per i file .md

## Sistema di equazioni

Per scrivere un sistema di equazioni si può utilizzare il seguente codice:

```latex
\begin{cases}
    equazione_1 \\
    equazione_2 \\
    ...
    equazione_n
\end{cases}
$$
```

Per inserire più sistemi di equazioni simile a questo:

$$
f = 10Hz \to
\begin{cases}
|Z_{est}| \approx 10^5 \Omega \\
|Z_{int}| \approx 10^11 \Omega
\end{cases}
\qquad
f= 1kHz
\begin{cases}
|Z_{est}| \approx 10^3 \Omega \\
|Z_{int}| \approx 10^9 \Omega
\end{cases}
\qquad
f=1MHz
\begin{cases}
|Z_{est}| \approx 1 \Omega \\
|Z_{int}| \approx 10^6 \Omega
\end{cases}$$

si può utilizzare il seguente codice:

```latex
$$
\begin{cases}
sistema_1
\end{cases}
\qquad
\begin{cases}
sistema_2
\end{cases}
\qquad
...
\begin{cases}
sistema_n
\end{cases}
$$
```

Il comando `qquad` è opzionale, in quanto serve solo per formattare il testo, inserendo uno spazio tra un sistema e l'altro, ma comunque consigliato.

## Tabelle

Per creare una tabella come la seguente:

| Caso ideale | $\mu$A741 |
|:-----------:|:----------:|
| $A_{OL} = \infty$ | $A_{OL} = 10^5$ (100dB)|
| $R_{IN} = \infty$ | $R_{IN} = 2M\Omega$ |
| $R_{O} = 0$ | $R_{O} = 25\Omega$ |
| Banda = $\infty$ | Banda = 1MHz |
| $CMRR = \infty$ | $CMRR = 90dB$ |
| $PGB =  \infty$ | $PGB = 1MHz$ |

si può utilizzare il seguente codice:

```latex
| testo_colonna1 | testo_colonna2 | ... | testo_colonnaN |
|:--------------:|:--------------:|:---:|:--------------:|
| riga1_colonna1 | riga1_colonna2 | ... | riga1_colonnaN |
| riga2_colonna1 | riga2_colonna2 | ... | riga2_colonnaN |
| ...            | ...            | ... | ...            |
| rigaN_colonna1 | rigaN_colonna2 | ... | rigaN_colonnaN |
```

## Immagini

Per inserire un'immagine si può utilizzare il seguente codice:

```latex
![testo_alternativo](path_immagine){opzioni grandeza}
```

dove:

- `testo_alternativo` è il testo che viene mostrato se l'immagine non viene caricata, e/o la descrizione dell'immagine;
- `path_immagine` è il percorso dell'immagine;
- `opzioni_grandezza` sono le opzioni per la grandezza dell'immagine. Quella consigliata è `width=N%`, che fa sì che l'immagine abbia una larghezza del N% (valore a piacere in base alla formattazione scelta) della larghezza della pagina.

## Link

Per inserire un link si può utilizzare il seguente codice:

```latex
[testo_link](link)
```
che restituisce un link cliccabile con il testo `testo_link` che rimanda al link `link`.

Può essere fatto anche per linkare parti del documento, utilizzando il seguente codice:

```latex
[testo_link](#nome_sezione)
```

dove `nome_sezione` è il nome della sezione a cui si vuole linkare.

## Lettere greche

Per inserire le lettere greche è sufficiente usare il carattere `\` seguito dal nome della lettera greca, all'interno di un blocco di `$`.  Per i non classicisti, ecco una tabella con tutto l'alfabeto greco, con maiuscole e minuscole, e relativi comandi:

| Nome | Maiuscolo | Minuscolo | Comando |
|:----:|:---------:|:---------:|:-------:|
| Alfa | $\Alpha$ | $\alpha$ | `\Alpha` `\alpha` |
| Beta | $\Beta$ | $\beta$ | `\Beta` `\beta` |
| Gamma | $\Gamma$ | $\gamma$ | `\Gamma` `\gamma` |
| Delta | $\Delta$ | $\delta$ | `\Delta` `\delta` |
| Epsilon | $\Epsilon$ | $\epsilon$ | `\Epsilon` `\epsilon` |
| Zeta | $\Zeta$ | $\zeta$ | `\Zeta` `\zeta` |
| Eta | $\Eta$ | $\eta$ | `\Eta` `\eta` |
| Theta | $\Theta$ | $\theta$ | `\Theta` `\theta` |
| Iota | $\Iota$ | $\iota$ | `\Iota` `\iota` |
| Kappa | $\Kappa$ | $\kappa$ | `\Kappa` `\kappa` |
| Lambda | $\Lambda$ | $\lambda$ | `\Lambda` `\lambda` |
| Mu | $\Mu$ | $\mu$ | `\Mu` `\mu` |
| Nu | $\Nu$ | $\nu$ | `\Nu` `\nu` |
| Xi | $\Xi$ | $\xi$ | `\Xi` `\xi` |
| Omicron | $\Omicron$ | $\omicron$ | `\Omicron` `\omicron` |
| Pi | $\Pi$ | $\pi$ | `\Pi` `\pi` |
| Rho | $\Rho$ | $\rho$ | `\Rho` `\rho` |
| Sigma | $\Sigma$ | $\sigma$ | `\Sigma` `\sigma` |
| Tau | $\Tau$ | $\tau$ | `\Tau` `\tau` |
| Upsilon | $\Upsilon$ | $\upsilon$ | `\Upsilon` `\upsilon` |
| Phi | $\Phi$ | $\phi$ | `\Phi` `\phi` |
| Chi | $\Chi$ | $\chi$ | `\Chi` `\chi` |
| Psi | $\Psi$ | $\psi$ | `\Psi` `\psi` |
| Omega | $\Omega$ | $\omega$ | `\Omega` `\omega` |

## Apici e pedici

Nel caso in cui l'apice o il pedice sia composto da un solo numero o carattere, è sufficiente inserirlo dopo il carattere `^` (apice) o `_` (pedice). Ad esempio:

```latex
$V_a^+$
$V_\alpha$
```

restituisce:

$V_a^+$
$V_\alpha$

Se invece l'apice o il pedice è composto da più caratteri, è necessario inserirli all'interno di `{}`.

## Simboli matematici

Una carrellata dei simboli matematici più utilizzati (sempre da usare all'interno di `$`):

- $\infty$ : `\infty`;
- $\approx$ : `\approx`;
- $\neq$ : `\neq`, oppure `\not=`;
- $\leq$ : `\leq`, oppure `\le`;
- $\geq$ : `\geq`, oppure `\ge`;
- $\pm$ : `\pm`;
- $\cdot$ : `\cdot` (inteso come prodotto);
- $\int$ : `\int` (per gli estremi si può usare `_` e `^`);
- $\sum$ : `\sum` (per gli estremi si può usare `_` e `^`);
- $\prod$ : `\prod` (per gli estremi si può usare `_` e `^`);
- $\sqrt{f(x)}$ : `\sqrt{f(x)}`;
- $\frac{a}{b}$ : `\frac{a}{b}`.
- $\binom{n}{k}$ : `\binom{n}{k}` (coefficiente binomiale);
- $\forall$ : `\forall`;
- $\exists$ : `\exists`;
- $\equiv$ : `\equiv`;
- $\partial$ : `\partial`;


## Matrici

Per inserire una matrice si può utilizzare il seguente codice:

```latex
\begin{matrix}
a & b \\
c & d
\end{matrix}
```

che restituisce:

$$
\begin{matrix}
a & b \\
c & d
\end{matrix}
$$

Per inserire una matrice con parentesi tonde si può utilizzare il seguente codice:

```latex
\begin{pmatrix}
a & b \\
c & d
\end{pmatrix}
```

che restituisce:

$$
\begin{pmatrix}
a & b \\
c & d
\end{pmatrix}
$$

## Limiti

Per inserire un limite si può utilizzare il seguente codice:

```latex
$\lim_{x \to 0} f(x)$
```

che restituisce:

$$
\lim_{x \to 0} f(x)
$$
