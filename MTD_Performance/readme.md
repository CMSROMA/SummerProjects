# Ntuple
`/afs/cern.ch/user/n/npalmeri/public/MTD_Performance`

- `MTD_ntuples_NoPU.root` contiene 2000 eventi $t\bar{t}$ di segnale puro.
- `MTD_ntuples_200PU.root` contiene 300 eventi $t\bar{t}$ con sovrapposta una media di 200 eventi di pile-up (PU).

# Informazioni generali
- Gli eventi sono $t\bar{t}$ con e senza 200 PU.
- Per la back-propagation delle tracce dal layer di MTD alla beamline, vengono considerate le sole ipotesi di pione, kaone e protone. 
- Vengono salvati **tutti** i vertici ricostruiti e simulati, sia di segnale che di pile-up, assieme alle relative tracce.

# Legenda variabili ntuple

## Event-Level

| **Variabile**  | **Tipo** | **Descrizione** |
|--|--|--|
| `run` | int 														|	Run ID |
| `event` | int 													| Event ID |
| `n_reco_tracks` | int 										| Numero totale di tracce ricostruite nell'evento|
| `n_reco_vtxs` | int 											| Numero totale di vertici ricostruiti nell'evento <br/> (= segnale + vertici di PU)|
| `n_sim_tracks` | int 										| Numero totale di tracce simulate nell'evento provenienti da vertici simulati |
| `n_sim_vtxs` | int 											| Numero totale di vertici simulati nell'evento <br/> (= segnale + vertici di PU)|
| `track_to_vertex_map` | std::map<int, int> | Mappa tra una traccia ricostruita e il suo vertice ricostruito corrispondente (usando `track_id`, `vtx_id`) |
| `reco_to_sim_vertex_map` | std::map<int, int> | Mappa che associa ad un vertice ricostruito il suo vertice simulato associato (usando `vtx_id`, `mc_vtx_id`)  |
| `reco_to_sim_track_map` | std::map<int, int> |  Mappa che associa ad una traccia ricostruita la sua traccia simulata associata (usando `track_id`, `mc_track_id`)  |
| `sim_track_to_sim_vertex_map` | std::map<int, int> | Mappa che associa ad una traccia simulata il suo vertice simulato associato (usando `mc_track_id`, `mc_vtx_id`) |

## Track-Level

Sistema di coordinate in CMS:
![Sistema di coordinate in CMS](https://tikz.net/wp-content/uploads/2023/10/axis3D_CMS-004.png)

**Glossario:**
- PCA = Point of Closest Approach. Per una traccia, rappresenta la sua intersezione con la linea di fascio, ovvero l'asse $z$ (e dunque anche il punto di massimo avvicinamento alla beamline).
- TP = Tracking Particle, qui usato come sinonimo per traccia MC, o anche traccia gen-level, o traccia gen.

### Reco

| **Variabile**  | **Tipo** | **Descrizione** |
|--|--|--|
| `track_id` | std::vector\<int\> | Identificativo della traccia nell'evento. |
| `track_pt` | std::vector\<float\> | Momento trasverso ($p_T$) della traccia |
| `track_eta` | std::vector\<float\> | Pseudorapidità ($\eta$) della traccia alla beamline|
| `track_phi` | std::vector\<float\> | Angolo polare ($\phi$) della traccia alla beamline |
| `track_PCAx` | std::vector\<float\> | Coordinata $x$ della traccia al PCA con la beamline |
| `track_PCAy` | std::vector\<float\> | Coordinata $y$ della traccia al PCA con la beamline |
| `track_PCAz` | std::vector\<float\> | Coordinata $z$ della traccia al PCA con la beamline |
| `track_MTDx` | std::vector\<float\> | Coordinata $x$ della traccia al layer di MTD |
| `track_MTDy` | std::vector\<float\> | Coordinata $y$ della traccia al layer di MTD |
| `track_MTDz` | std::vector\<float\> | Coordinata $z$ della traccia al layer di MTD |
| `track_tmtd` | std::vector\<float\> | Tempo della traccia al layer di MTD |
| `track_sigmatmtd` | std::vector\<float\> | Incertezza sul tempo della traccia al layer di MTD |
| `track_t0` | std::vector\<float\> | Tempo alla beamline della traccia <br/>(= tempo ad MTD - tempo di volo secondo ipotesi più probabile) |
| `track_sigmat0` | std::vector\<float\> | Incertezza su tempo della traccia alla beamline |
| `track_tofPi` | std::vector\<float\> | Tempo di volo della particella sotto ipotesi di pione |
| `track_tofK` | std::vector\<float\> | Tempo di volo della particella sotto ipotesi di kaone |
| `track_tofP` | std::vector\<float\> | Tempo di volo della particella sotto ipotesi di protone |
| `track_sigmatofPi` | std::vector\<float\> | Incertezza sul tempo di volo della particella sotto ipotesi di pione |
| `track_sigmatofK` | std::vector\<float\> |  Incertezza sul tempo di volo della particella sotto ipotesi di kaone |
| `track_sigmatofP` | std::vector\<float\> |  Incertezza sul tempo di volo della particella sotto ipotesi di protone |
| `track_probPi` | std::vector\<float\> | Probabilità PID calcolata per ipotesi di pione |
| `track_probK` | std::vector\<float\> | Probabilità PID calcolata per ipotesi di kaone |
| `track_probP` | std::vector\<float\> | Probabilità PID calcolata per ipotesi di protone |
| `track_mtdMVAflag` | std::vector\<float\> | Flag di qualità di associazione traccia-hit MTD calcolata da una BDT (a valori tra 0 e 1). |
| `track_vtx3Dwgt` | std::vector\<float\> | Peso della traccia associata al relativo vertice ottenuta dal fit 3D del vertice. <br/> (valuta la qualità dell'associazione e approx. la probabilità che la traccia provenga effettivamente a quel vertice) |
| `track_match_category` | std::vector\<float\> | Flag sul matching della traccia con la relativa traccia MC <br/> - matchCategory = -1: traccia non associata a TP (fake track) <br/> - matchCategory = 0: traccia associata ad una TP di una qualche vertice simulato dell'evento <br/> - matchCategory = 1: traccia associata ad una TP dello stesso evento di un vertice simulato, ma non al vertice stesso (traccia secondaria) <br/> - matchCategory = 2: traccia associata ad una TP non proveniente da nessuno dei vertici simulati (traccia di PU)|
| `track_MVA_selected` | std::vector\<bool\> | Flag di qualità vera se la traccia passa i seguenti tagli:<br/>- $p_T > 0.7\, \text{GeV}$ <br/> - $\Delta Z(\text{track, vtx}) \leq 1\,\text{mm}$ <br/> - $\mid{\eta}\mid < 3$ <br/> - $\Delta t (\text{track, vtx}) < 3 \sigma_t$ |

### MC
| **Variabile**  | **Tipo** | **Descrizione** |
|--|--|--|
| `mc_track_id` | std::vector\<int\> | Identificativo della traccia simulata nell'evento. |
| `mc_track_pt` | std::vector\<float\> | Momento trasverso ($p_T$) della traccia simulata. | 
| `mc_track_eta` | std::vector\<float\> | Pseudorapidità ($\eta$) della traccia simulata. alla beamline |
| `mc_track_phi` | std::vector\<float\> | Angolo polare ($\phi$) della traccia simulata alla beamline |
| `mc_track_PCAx` | std::vector\<float\> | Coordinata $x$ al PCA alla beamline della traccia MC |
| `mc_track_PCAy` | std::vector\<float\> |  Coordinata $y$ al PCA alla beamline della traccia MC |
| `mc_track_PCAz` | std::vector\<float\> |  Coordinata $z$ al PCA alla beamline della traccia MC |
| `mc_track_MTDx` | std::vector\<float\> | Coordinata $x$ al layer di MTD della traccia MC |
| `mc_track_MTDy` | std::vector\<float\> | Coordinata $y$ al layer di MTD della traccia MC |
| `mc_track_MTDz` | std::vector\<float\> | Coordinata $z$ al layer di MTD della traccia MC |
| `mc_track_tmtd` | std::vector\<float\> | Tempo al layer MTD della traccia MC |
| `mc_track_t0` | std::vector\<float\> | Tempo alla beamline della traccia MC <br/> (== tempo del vertice simulato associato) |
| `mc_track_pdgId` | std::vector\<float\> | Vera identità della particella usando il numbering scheme Monte Carlo del PDG <br/> (vedi [questa tabella](https://pdg.lbl.gov/2024/mcdata/mc_particle_id_contents.html)) |

## Vertex-Level

### Reco
| **Variabile**  | **Tipo** | **Descrizione** |
|--|--|--|
| `vtx_id` | std::vector\<int\> | Identificatore del vertice |
| `is_signal_vtx` | std::vector\<bool\> | Flag che stabilisce se il vertice è di segnale (i.e. leading vertex).  |
| `n_tracks_per_vertex` | std::vector\<int\> | Numero di tracce associate al vertice (stabilito dalla ricostruzione del vertice). |
| `vtx_x` | std::vector\<float\> | Coordinata $x$ del vertice ricostruito. |
| `vtx_y` | std::vector\<float\> | Coordinata $y$ del vertice ricostruito. |
| `vtx_z` | std::vector\<float\> | Coordinata $z$ del vertice ricostruito. |
| `vtx_sigmaz` | std::vector\<float\> | Incertezza sulla coordinata $z$ del vertice ricostruito. |
| `vtx_t` | std::vector\<float\> | Tempo ricostruito del vertice. |
| `vtx_sigmat` | std::vector\<float\> | Incertezza sul tempo ricostruito del vertice. |

### MC

| **Variabile**  | **Tipo** | **Descrizione** |
|--|--|--|
| `mc_vtx_id` | std::vector\<int\> | Identificatore del vertice simulato. |
| `mc_is_signal_vtx` | std::vector\<bool\> | Se il vertice simulato corrente è di segnale o no (PU). |
| `mc_n_tracks_per_vertex` | std::vector\<int\> | Numero di tracce simulate associate al vertice simulato. |
| `mc_vtx_t` | std::vector\<float\> | Tempo del vertice simulato. |
| `mc_vtx_x` | std::vector\<float\> | Coordinata $x$ del vertice simulato. |
| `mc_vtx_y` | std::vector\<float\> | Coordinata $y$ del vertice simulato. |
| `mc_vtx_z` | std::vector\<float\> | Coordinata $z$ del vertice simulato. |