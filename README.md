# 21741 - Arquitectures Avançades

## [***Petri Nets***](https://github.com/aNDREUET648/aa.aa_PetriNets)


## Taula de continguts
- [Introducció](#introducció)
- [Enunciat](#enunciat)
- [Requisits mínims](#requisits-mínims)
- [Xarxa de Petri](#xarxa-de-petri)
- [Consideracions](#consideracions)
- [Bibliografia i Eines](#bibliografia-i-eines)

### Introducció

   El ràpid desenvolupament del *Deep Learning* ha fet que ens adonem que els processadors originals ja no poden realitzar càlculs específics a gran escala amb la premura que requerim.

   En 1965, Gordon Moore va predir que el nombre de transistors per xip es duplicaria cada un o dos anys, ja no comptam amb l'*Escala de Dennard* per que ja no s'aplica. Ja em substituït el processador únic per múltiples nuclis, però encara així les millores cost-rendiment i eficiència energètica per a arquitectures de propòsit general és limitat (límits d'electromigració, mecànics i tèrmics dels xips). 

   Si volem un major rendiment (més operacions/segon), necessitam reduir l'energia/operació i a la vegada augmentar el nombre d'operacions aritmètiques/instrucció d'una a centenars. Aquesta desesperació per aconseguir aquest nivell d'eficiència és la raó pel que els arquitectes han realitzat un canvi dràstic en l'arquitectura dels ordinadors passant dels nuclis de propòsit general a les *Arquitectures de Domini Específic (DSA)*.
   
   Aquesta nova normalitat és que un ordinador estigui format per processadors estàndard per a executar programes convencionals, com els sistemes operatius, junt amb *processadors específics de domini* que només realitzin un rang estret de tasques, però que les fa extremadament ràpid i bé. Per tant, aquests ordinadors seran molt més heterogenis que els xips multinucli homogenis del passat.
   
   Un disseny representatiu en materia de DSA és la *[Unitat de Processament Tensorial de Google (Tensor Processing Unit)](https://en.wikipedia.org/wiki/Tensor_Processing_Unit)*. La Unitat de Processament Tensorial (TPU) es va implantar per primera vegada al 2015 i actualment proporciona serveis a més de mil milions de persones. La TPU fa servir una unitat d'acceleració de càlcul matricial basat en un disseny de matriu sistòlica que accelera el càlcul de les Xarxes Neuronals Profundes (Deep Neural Networks. DNN) entre 15 i 30 vegades més ràpid i amb una eficiència energètica entre 30 i 80 vegades superior a les CPUs actual i les GPUs de tecnologies similars.

   Si bé encara segueix essent vàlida, la *Llei de Moore* està arribant a la seva fi, deixant a les *Arquitectures de Domini Específic (DSA)* el pes del futur de la informàtica.
      

---

### Enunciat

Implementació d'una Xarxa de Petri per a modelar l'arquitectura d'una *Unitat de Processament Tensorial de Google*.

El document [Arquitectura de la TPU](./DSA-TPU_architecture.pdf) recull tota la informació aconseguida per a la realització de la pràctica. 

![Diagrama de Blocs](./imatges/tpu_block_diagram.png?raw=true "Diagrama de Blocs de la TPU")

---
### Requisits mínims

Per a poder executar la pràctica cal instal·lar l'eina PIPE 2.7 *(Platform-Independent Petri net Editor)* i seguir les següents pases:
  - Descarregar i descomprimir l'arxiu comprimit *.zip* [```Pipe 2.7```](./pipe2.7/pipe2.7%5B20131028%5D.zip).  
  - Descarregar també l'arxiu [```TPU ARCHITECTURE v.2.3.xml```](./TPU%20ARCHITECTURE%20v.2.3.xml). Aquesta és la pràctica en qüestió. La Xarxa de Petri a analitzar. Podeu clicar aquest enllaç o adalt de tot teniu l'arxiu disponible també per a descarregar.
  - Per a l'execució de la versió *Windows*, executar s'arxiu ```pipe.bat```
  - Per a l'execució de la versió *linux*, executar ```pipe.sh```
  - Obriu l'arxiu ```TPU ARCHITECTURE v.2.3.xml```

---
### Xarxa de Petri

Una representació de la Xarxa de Petri per a la nostra TPU és la següent:

![Xarxa de Petri](./imatges/tpu_petri_net_v2.3.PNG?raw=true "Xarxa de Petri de la TPU")

---

### Consideracions


El model en el que basaré la pràctica és la TPUv1 on he trobat tota la informació.

Implementar una TPU completa pot ser una feina complicada. Per tal de reduir sa càrrega de treball i millorar sa viabilitat es requeriran una sèrie de simplificacions per a la TPU. Totes les simplificacions han de ser tal que, no han de perdre el concepte de disseny de la pròpia TPU.

Degut a que el **Control** és la part que menys comentada, centraré el focus d'atenció de la Xarxa de Petri en aquesta direcció, i més concretament en l'execució de les instruccions que impliquen l'execució de la resta de blocs de la *TPU*.

Per a mantenir una coherència amb el diagrama de blocs de l'[Enunciat](#enunciat), les etiquetes dels *Llocs* i de les *Transicions* tendran un nom similar, sino que hi haurà, que seran el mateix.

A una Xarxa de Petri Estocàstica Generalitzada (GSPN) per un marcat donat, diverses transicions poden estar simultàniament habilitades, i si s'incloeixen transicions inmediates i temporitzades, només les transicions inmediates poden disparar-se perquè tenen una prioritat més alta que les temporitzades.

La càrrega d'instruccions que vagin arribant es farà a través d'una transició *source* que es diu *Load Instruction* i és de tipus temporatizat (de rati=0.25). La resta de transicions són inmediates. La meva intenció hagués estat afegir dues transicions temporitzades a on es facin crides *Off-Chip* aquestes són *Load Weights* i *DMA Controller. Write to Host*, però em trob que hi ha marcats a on es solapen amb transicions inmediates i aquestes dues no s'arriben a disparar mai.

El punts clau que he tret del document [Arquitectura de la TPU](./DSA-TPU_architecture.pdf) són els següents:
   - Thus each of the preceding four general categories of instructions have separate execution hardware (with read and write host memory combined into the same unit).
   - The Matrix Multiply Unit has not-ready signals from the Unified Buffer and the Weight FIFO that will cause the Matrix Multiply Unit to stall if the input activation or weight data are not yet available.
   - The TPU does not have a program counter, and it has no branch instructions; instructions are sent from the host CPU.
   - To increase instruction parallelism further, toward that end, the Read_Weights instruction follows the decoupled access/execute philosophy in that they can complete after sending its address but before the weights are fetched from Weight Memory.

El tractament de cada una de les 4 categories d'instruccions és independent
   - ```Read_Weights``` llegirà els pesos del ```Weight Memory```, els carregarà a la ```Weight FIFO``` a l'espera per començar a operar-les.
   - ```Matrix_Multiply/Convolve``` és una instrucció de doble, i té distint comportament depenguent del que hagi de fer:
      - *Convolve*: collirà les dades del *Unified Buffer* i les ficarà als *Accumulators* per a després operar. 
      - *Multiply*: operació encarregada en multiplicar a la *Matrix Multiply Unit* les dades que hi ha al *Unified Buffer* amb els pesos que hi ha a la *Weight FIFO*. No entraré en detalls si les multiplicacions són de vectors, matrius o parts de la matriu.
   - ```Activate``` realitza les funcions no lineals de la neurona artificial (ReLU, Sigmoide, tanh, etc). Les seves entrades són els *Accumulators* i la seva sortida va cap el *Unified Buffer*. Aquí és on també es realitzen les operacions necessàries d'agrupació per a les convolucions ja que està connectat amb la lògica de la funció no lineal.
   - ```Read_Host_Memory``` i ```Write_Host_Memory``` són tractades com a una mateixa categoria, a on el *Host Interface* és el seu nexe de unió (Xarxa de Petri *Lector-Escriptor*).

Mitjançant la transició *Systolic Control* controlarem que tenim carregats els pesos, les dades d'activació d'entrada al *Unified Buffer* i tenim una operació de *Multiply* a punt per executar la *Matrix Multiply Unit*.

La transició *Read_Host_Memory* estarà habilitada si el llocs *CPU Host Memory* i *Host Interface* estan disponibles i tenim una instrucció al *Instruction Buffer*. Només així podrem carregar les dades d'activació d'entrada, i no serà fins que la transició *DMA Controller. Load from Host* hagi carregat aquestes dades al *Unified Buffer* que no tornarem a tenir el *Host Interface* disponible.

Per una altra banda *Write_Host Memory* estarà habilitada quan el *Host Interface* estigui disponible, tenguem una instrucció al *Instruction Buffer* i s'hagin tractat les dades a *Compute. Activation*. A partir d'aquest *Host Interface* estarà ocupat i no tornarà estar lliure fins al moment d'escriure les dades al *CPU Host Memory* mitjançant la transició *DMA Controller. Write to Host* i només passarà si són enviades al *Unified Buffer* a través de la transició *Send to Unibuffer*.

 

---

### Bibliografia i Eines

  - [Computer Architecture: A Quantitative Approach](https://www.elsevier.com/books/computer-architecture/hennessy/978-0-12-811905-1) - Chapter 7. Domain-Specific Architectures
  - [A domain-specific architecture for deep neural networks](https://dl.acm.org/doi/pdf/10.1145/3154484) - Communications of the ACM, September 2018, Vol. 61 No. 9, Pages 50-59
  - [Google Cloud TPU](https://cloud.google.com/tpu) - Pàgina presentació de Google TPU. Inclou sa Demo/Presentació NEXT TPU
  - [Petri Nets. Properties, Analysis and Applications](./Murata%20-%20Petri%20Nets%20-%20Properties%2C%20Analysis%20and%20Applications.pdf) - Proceedings of the IEEE ( Volume: 77, Issue: 4, April 1989). Tadao Murata
  - [Petri Net Theory and the Modeling of Systems](https://www.amazon.es/Petri-Net-Theory-Modeling-Systems/dp/1080591176) - Chapters 1, 2 and 3. James L. Peterson
  - Documentació de classe del Tema 4 - Arquitectures de Domini Específiques - Assignatura. Arquitectures Avançades. Cati Lladó
  - [PIPE v2.5: a Petri Net Tool for Performance Modeling](https://www.doc.ic.ac.uk/~wjk/publications/bonet-llado-knottenbelt-puijaner-clei-2007.pdf) - P. Bonet, C. Lladó, R. Puigjaner and W. Knottenbelt


---
[aNDREUET648](https://github.com/aNDREUET648)
