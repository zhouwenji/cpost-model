# 2. Mathematical Framework

This section presents the model’s optimization framework and the mathematical principles.

## 2.1. Sets and Indices

The COPST model is mathematically formulated as a mixed integer linear programming problem (MILP). For some cases that need faster computation, the model could also be simplified to regular linear programming by changing configuration settings and ignoring binary variables, as in unit commitment. {ref}`Table 2-1 <Table 2-1>` below summarizes the core concepts involved in the mathematical programming framework, which are essential for the computational implementation.

```{table} Table 2-1: Symbols and their explanations.
:name: Table 2-1

| Symbol               | Explanation |
|----------------------|---------------|
| $n,n'\in N$          | Nodes          | 
| $p\in P$             | Processes (e.g., generation technologies)          |
| $u\in U$             | Generation units          | 
| $t\in T$             | Time step (hour)          |
| $c\in C$             | Commodities (e.g., electricity, hydrogen, methane, CO<sub>2</sub>, etc.)          | 
| $s\in S$             | Storage installations (e.g., batteries for electricity storage, pumped hydro, hydrogen storage tanks, etc.)       |
| $f\in F$             | Transmission lines (e.g., high-voltage transmission lines for electricity, hydrogen pipelines for hydrogen, etc.) | 
| $C_{\text{com}},C_{\text{dem}},C_{\text{buy}},C_{\text{sell}},C_{\text{emi}}\subset C$ | Subsets of commodities, where:<br> - $C_{\text{com}}$: regular commodities, e.g., electricity, hydrogen, and others;<br>- $C_{\text{dem}}$: demand commodities;<br>- $C_{\text{buy}},\ C_{\text{sell}}$: commodities to buy and sell, incl. imported electricity, gas purchased from the market, electricity, hydrogen sold to the market, etc.;<br>- $C_{\text{emi}}$: emissions, e.g.: CO<sub>2</sub>, SO<sub>2</sub>, etc. |
| $(n,p,u)\in NPU$     | Combination set, unit $u$ of process $p$ at node $n$          | 
| $NPU^{\text{noren}}, NPU^{\text{ren}}\subset NPU$ | Sub-combination set, non-renewable unit $u$ process $p$ at node $n$, and renewable unit $u$ process $p$ at node $n$          |
| $(n,s,c)\in NSC$     | Combination set, storage $s$ of commodity $c$ at node $n$          | 
| $(n,n',f,c)\in NNFC$ | Combination sets, transmission $f$ of commodity $c$ from node $n$ to node $n’$          | 
```


{ref}`Table 2-2 <Table 2-2>` provides an overview of the key parameters in the model.

```{table} Table 2-2: Parameters and their explanations.
:name: Table 2-2

| Parameter            | Explanation |
|----------------------|---------------|
| $fixedcost_{n,p,u}$ | Fixed cost of unit $u$ of process $p$ at node $n$ |
| $varcost_{n,p,u}$ | Variable cost of unit $u$ of process $p$ at node $n$ |
| $startcost_{n,p,u}$ | Start-up cost of unit $u$ of process $p$ at node $n$ |
| $ramp_{n,p,u}^{\text{up}}$ | Ramp-up rate of unit $u$ of process $p$ at node $n$, in percentage of capacity |
| $ramp_{n,p,u}^{\text{down}}$ | Ramp-down rate of unit $u$ of process $p$ at node $n$, in percentage of capacity |
| $cap_{n,p,u}$ | Capacity of unit $u$ of process $p$ at node $n$ |
| $capfactor_{n,p,u}$ | Capacity factor of unit $u$ of process $p$ at node $n$ |
| $mincap_{n,p,u}$ | Minimum capacity of unit $u$ of process $p$ at node $n$ when it is operating |
| $rate_{n,p,u,c}^{\text{in}}$ | Inflow rate coefficient of commodity $c$ to unit $u$ of process $p$ at node $n$ |
| $rate_{n,p,u,c}^{\text{out}}$ | Outflow rate coefficient of commodity $c$ to unit $u$ of process $p$ at node $n$ |
| $storcap_{n,s,c}^{\text{v}}$ | Volume capacity of storing commodity $c$ in storage $s$ at node $n$, in the unit of energy (e.g., kWh or MJ) |
| $storcap_{n,s,c}^{\text{in}}$ | Inflow (charging) capacity of storing commodity $c$ in storage $s$ at node $n$, in the unit of power (e.g., kW) |
| $storcap_{n,s,c}^{\text{out}}$ | Outflow (discharging) capacity of storing commodity $c$ in storage $s$ at node $n$, in the unit of power (e.g., kW) |
| $storinit_{n,s,c}$ | Initial status of storing commodity $c$ in storage $s$ at node $n$, percentage |
| $storvarcost_{n,s,c}$ | Variable cost of storing commodity $c$ in storage $s$ at node $n$ |
| $storeff_{n,s,c}^{\text{in}}$ | Input (charging) efficiency of commodity $c$ in storage $s$ at node $n$, percentage |
| $storeff_{n,s,c}^{\text{out}}$ | Output (discharging) efficiency of commodity $c$ in storage $s$ at node $n$, percentage |
| $selfdischarge_{n,s,c}$ | Self-discharging (loss) rate of commodity $c$ flowing out of storage $s$ at node $n$, percentage |
| $transcap_{n,n',f,c}$ | Transmission capacity of storing commodity $c$ in storage $s$ at node $n$, in the unit of power (e.g., kW) |
| $transvarcost_{n,n',f,c}$ | Variable cost of transmitting commodity $c$ via transmission line $f$ from node $n$ to node $n’$ |
| $transeff_{n,n',f,c}$ | Efficiency for transmitting commodity $c$ via line $f$ from node $n$ to node $n’$, percentage (100% - line loss) |
| $intermittent_{n,p,t}$ | Conversion factor of intermittent energy process $p$ at time step $t$ at node $n$ |
| $price_{n,c,t}$ | Price of commodity $c$ at time step $t$ at node $n$ |
| $demand_{n,c,t}$ | Demand of commodity $c$ at time step $t$ at node $n$ |
```

{ref}`Table 2-3 <Table 2-3>` lists the variables in the model that are directly associated with the decision-making process:

```{table} Table 2-3: Variables and their explanations.
:name: Table 2-3

| Variable            | Explanation |
|----------------------|---------------|
| $ACT_{n,p,u,t}$ | Activity (generation or output) of unit $u$ of process $p$ at time step $t$ at node $n$ |
| $ON_{n,p,u,t}$ | Binary variable, operating status of unit $u$ of process $p$ at time step $t$ at node $n$, 1: on; 0: off |
| $SWITCH_{n,p,u,t}$ | Binary variable, action of unit $u$ of process $p$ at time step $t$ at node $n$, 1: switch on |
| $STORCOM_{n,s,c,t}$ | Stored amount of commodity $c$ in storage $s$ at time step $t$ at node $n$ |
| $STORIN_{n,s,c,t}$ | Inflow (charging) rate of commodity $c$ in storage installation $s$ at time step $t$ at node $n$ |
| $STOROUT_{n,s,c,t}$ | Outflow (discharging) rate of commodity $c$ in storage $s$ at time step $t$ at node $n$ |
| $TRAACT_{n,n',f,c,t}$ | Flow rate of commodity $c$ via transmission $f$ at time step $t$ from node $n$ to node $n’$ |
```

## 2.2. Production Process

The production process in CPOST captures technologies’ input-output conversion relationships. This relationship can represent various technologies, including power generation, transmission, and storage. Each process is treated with predefined parameters governing its inflows and outflows. The process supports the representation of diverse energy carriers beyond electricity and enables modeling sector-coupling (e.g., hydrogen, methane) in a consistent framework.

```{figure} _static/fig_2_1.png
---
name: Fig. 2-1
align: center
width: 70%
---
Fig. 2-1: Representation of the inflows and outflows of a coal-fired generation unit.
```

{ref}`Fig. 2-1 <Fig. 2-1>` illustrates a coal-fueled power plant and the associated input and output flows. The generator $G$ has coal as the input fuel and electricity as the primary output product, while it also emits CO<sub>2</sub> as the secondary output commodity. Three parameters need to be defined then: $rate^{\text{in}}_{{G,coal}}$, $rate^{\text{out}}_{{G,elec}}$, and $rate^{\text{in}}_{{G,CO_2}}$, denoting the inflow rate coefficient for coal, the outflow rate coefficients for electricity and CO<sub>2</sub>, respectively.  Given these three parameters, the model defines only one decision variable, $ACT_G$, to represent the activity level of generator $G$ in a specific circumstance. The electricity output is therefore calculated as $rate^{\text{out}}_{{G,elec}}\times ACT_G$, and similarly, the inflow of coal and the CO<sub>2</sub> emissions are $rate^{\text{in}}_{{G,coal}}\times ACT_G$ and $rate^{\text{out}}_{{G,CO_2}}\times ACT_G$, respectively. 


## 2.3. Objective Function

The objective function is to minimize the total cost of the whole electricity system consisting of a variety of components, including capital investments, fixed & variable costs, storage costs, transmission costs, the purchase cost of fuels, the income from selling the produced commodities (as negative costs in the function), and the penalty from emissions. The overall objective function is expressed as:

$$
totalcost = \min (\zeta_{\text{fixed}} + \zeta_{\text{var}} + \zeta_{\text{stor}} + \zeta_{\text{trans}} + \zeta_{\text{buy}} + \zeta_{\text{sell}} + \zeta_{\text{emi}})\tag{2-1}
$$

where the total fixed cost $\zeta_{\text{fixed}}$ is the sum of all the fixed costs of operating units:

$$
\zeta_{\text{fixed}} = \sum_{t} \sum_{(n,p,u)\in NPU} fixedcost_{n,p,u}\cdot ON_{n,p,u,t}\tag{2-2}
$$

The total variable cost $\zeta_{\text{var}}$ is the sum of the variable costs for all units. For each unit u, its variable cost is calculated by multiplying the unit variable cost ($/kW) by the generation amount (activities) of each unit (kW) plus the start-up cost if the unit is switched on:

$$
\zeta_{\text{var}} = \sum_{t} \sum_{(n,p,u)\in NPU}(varcost_{n,p,u}\cdot ACT_{n,p,u,t} + startcost_{n,p,u}\cdot SWITCH_{n,p,u,t})\tag{2-3}
$$

The total storage cost $\zeta_{\text{stor}}$ is the sum of the storage costs for all the storage installations. For each installation $s$, the cost is calculated by multiplying the unit variable storage cost by the inflow rate of commodity $c$ into the installation. 

$$
\zeta_{\text{stor}} = \sum_{t} \sum_{(n,s,c)\in NSC} storvarcost_{n,s,c}\cdot STORIN_{n,s,c,t}\tag{2-4}
$$

The total transmission cost $\zeta_{\text{trans}}$ is the sum of the transmission costs for all the transmission lines. For each line $f$, the cost is calculated by multiplying the unit variable transmission cost by the flow rate of commodity $c$ via the line. 

$$
\zeta_{\text{trans}} = \sum_{t} \sum_{(n,n',f,c)\in NNFC} transvarcost_{n,s,c}\cdot TRAACT_{n,n',f,c,t}\tag{2-5}
$$

The total purchasing cost $\zeta_{\text{buy}}$ is the sum of the fuel costs for all the commodities flowing into all the units. For each installation $f$, the fuel cost is calculated by multiplying the unit price of commodity $c$ with its inflow amounts. Note that the input amounts of commodity $c$ flowing into generation unit $u$ is the product of its inflow rate coefficient $rate^{\text{in}}$ (parameter) and $ACT$ of generation unit $u$ (decision variable). The same rule applies to outflow calculation.

$$
\zeta_{\text{buy}} = \sum_{t} \sum_{(n,p,u)\in NPU} \sum_{c\in C_{\text{buy}}} price_{n,c,t}\cdot rate_{n,p,u,v}^{\text{in}}\cdot ACT_{n,p,u,t}\tag{2-6}
$$

The total income $\zeta_{\text{sell}}$ from selling the product commodity $c$ is calculated by multiplying the unit price of this product by its output. Note that this item is negative:

$$
\zeta_{\text{sell}} = -\sum_{t} \sum_{(n,p,u)}\sum_{c\in C_{\text{sell}}} price_{n,c,t}\cdot rate_{n,p,u,v}^{\text{out}}\cdot ACT_{n,p,u,t}\tag{2-7}
$$

Finally, the total emission cost $\zeta_{\text{emi}}$ for emitting commodity $c$ is calculated by multiplying the unit price of this pollutant (e.g., carbon tax or carbon market price) by its emissions:

$$
\zeta_{\text{emi}} = \sum_{t} \sum_{(n,p,u)\in NPU} \sum_{c\in C_{\text{emi}}} price_{n,c,t}\cdot rate_{n,p,u,v}^{\text{out}}\cdot ACT_{n,p,u,t}\tag{2-8}
$$

## 2.4. Constraints

The model optimizes the objective function under a set of constraints. These constraints are set for commodity balance (ensures that at each time step $t$ within each node $n$, the total supply of each regular commodity $c$ equals its total consumption); Unit commitment constraints (ensure that the operating status and the action variable take an appropriate value when units start-up and changes status from the previous time step $t-1$); Capacity constraints (limit the activity for each operating unit within the range between the minimum capacity required for keeping the operating status and the maximum capacity of the unit); Storage constraints (ensure that the charging/discharging speed cannot exceed its charging/discharging capacity (or speed limit); Transmission constraints (ensures that the transmission flow of commodity $c$ could not exceed the capacity of transmission line with the account of the line loss).

### 2.4.1. Commodity Balance

The commodity balance ensures that at each time step $t$ within each node $n$, the total supply of each regular commodity $c$ equals its total consumption. The left-hand side of Eq. (2-9) is the total supply of commodity $c$, containing the outputs of this commodity (outflow) produced from all the generation units, the transmitted amounts from all other nodes to node $n$, and the discharged amounts from all the storage installations. The right-hand side is the total consumption of commodity $c$, including its final demand at node $n$, the amounts consumed for all the generation units, the quantities transmitted from node $n$ to all other nodes, and the amounts flowing into all the storage installations. This balance holds for every regular commodity $c$ at every time step $t$ within every node $n$.

\begin{equation*}
\begin{aligned}
&\sum_{(n,p,u)\in NPU} rate_{n,p,u,c}^{\text{out}}\cdot ACT_{n,p,u,t} + \sum_{(n',n,f,c)\in NNFC} transeff_{n',n,f,c}\cdot TRAACT_{n',n,f,c,t} \\
&+ \sum_{(n,s,c)\in NSC} storeff_{n,s,c}^{\text{dis}}\cdot STOROUT_{n,s,c,t}\\
&= demand_{n,c,t} + \sum_{(n,p,u)\in NPU} rate_{n,p,u,c}^{\text{in}}\cdot ACT_{n,p,u,t} + \sum_{(n',n,f,c)\in NNFC} TRAACT_{n',n,f,c,t}\\
&+ \sum_{(n,s,c)\in NSC} storeff_{n,s,c}^{\text{ch}}\cdot STORIN_{n,s,c,t},\ \forall n\in N,\ t\in T,\ c\in C_{\text{com}}
\end{aligned}
\tag{2-9}
\end{equation*}

### 2.4.2. Unit Commitment Constraints

Constraints (2-10)-(2-12) ensure that the operating status $ON$ and the action variable $SWITCH$ take an appropriate value $(0,1)$ when unit $u$ starts up and changes status from the previous time step $t-1$.

$$
SWITCH_{n,p,u,t}\leq ON_{n,p,u,t}\quad \forall t\in T,\ \forall (n,p,u)\in NPU\tag{2-10}
$$

$$
SWITCH_{n,p,u,t}\leq 1-ON_{n,p,u,t-1}\quad \forall t\neq 1,\ \forall (n,p,u)\in NPU\tag{2-11}
$$

$$
ON_{n,p,u,t} - ON_{n,p,u,t-1}\leq SWITCH_{n,p,u,t}\quad \forall t\neq 1,\ \forall (n,p,u)\in NPU\tag{2-12}
$$

Constrain (2-13) ensures the change rate of activity for unit $u$ falls within the range of ramp-up and ramp-down speed limits.

$$
-ramp_{n,p,u}^{\text{down}}\cdot cap_{n,p,u}\leq ACT_{n,p,u,t}-ACT_{n,p,u,t-1}\leq ramp_{n,p,u}^{\text{up}}\cdot cap_{n,p,u}\quad \forall t\neq 1,\ \forall (n,p,u)\in NPU\tag{2-13}
$$

### 2.4.3. Capacity Constraints

Constraint (2-14) limits the activity for each operating unit $u$ within the range between the minimum capacity required to keep the operating status and the unit’s maximum capacity.
	 
$$
mincap_{n,p,u}\cdot ON_{n,p,u,t}\leq ACT_{n,p,u,t}\leq cap_{n,p,u}\cdot capfactor_{n,p,u}\cdot ON_{n,p,u,t}\quad \forall t\in T,\forall (n,p,u)\in NPU^{\text{noren}}\tag{2-14}
$$

Constraint (2-15) ensures that, for non-dispatchable renewable unit $u$, the unit runs at its total capacity if the capacity factor at time step $t$ is larger than the minimum capacity requirement. Otherwise, the unit is shut down.

\begin{equation*}
    \mathrm{ACT}_{n,p,u,t} =
    \begin{cases}
        cap_{n,p,u} \cdot intermittent_{n,p,t} \cdot ON_{n,p,u,t}, & 
        \text{if } cap_{n,p,u} \cdot intermittent_{n,p,t} > mincap_{n,p,u} \\
        0, & \text{otherwise}
    \end{cases}\\
    \tag{2-15}
\end{equation*}

$$\forall t \in T, \ \forall (n,p,u) \in NPU^{\text{ren}}$$

### 2.4.4. Storage Constraints

Constraints (2-16) and (2-17) ensure that the charging/discharging speed (inflow/outflow rate) of commodity $c$ into/from storage installation $s$ can not exceed its charging/discharging capacity (or speed limit).

$$
STORIN_{n,s,c,t}\leq storcap_{n,s,c}^{\text{in}}\quad \forall t\in T,\forall (n,s,c)\in NSC\tag{2-16}
$$

$$
STOROUT_{n,s,c,t}\leq storcap_{n,s,c}^{\text{out}}\quad \forall t\in T,\forall (n,s,c)\in NSC\tag{2-17}
$$

Constraint (2-18) ensures that the stored amount of commodity $c$ in installation $s$ can not exceed the volume capacity of $s$.

$$
STORCOM_{n,s,c,t}\leq storcap_{n,s,c}^{\text{v}}\quad \forall t\in T,\forall (n,s,c)\in NSC\tag{2-18}
$$

Constraint (2-19) calculates the initial status of commodity $c$ in $s$ when $t=1$.

$$
STORCOM_{n,s,c,t} = storcap_{n,s,c}^{\text{v}}\cdot storinit_{n,s,c}\quad \forall t=1,\forall (n,s,c)\in NSC\tag{2-19}
$$

Constraint (2-20) calculates the energy status change of commodity $c$ in $s$ at time step $t$ from $t-1$, by taking account of the inflow, the outflow at $t$, and the natural energy loss during the time interval.

\begin{align*}
STORCOM_{n,s,c,t} 
&= (1-selfdischarge_{n,s,c})\cdot STORCOM_{n,s,c,t}\\
&+ storeff_{n,s,c}^{\text{in}}\cdot STORIN_{n,s,c,t}\\
&- storeff_{n,s,c}^{\text{out}}\cdot STOROUT_{n,s,c,t}\\
& \forall t\neq 1,\forall (n,s,c)\in NSC\tag{2-20}
\end{align*}


### 2.4.5. Transmission Constraints

Constraint (2-20) ensures that the transmission flow of commodity $c$ could not exceed the capacity of transmission line $f$, with account the line loss.

$$
TRAACT_{n,n',f,c,t} \leq transeff_{n,n',f,c}\cdot transcap_{n,n',f,c}\quad \forall t\in T,\forall (n,n',f,c)\in NNFC\tag{2-20}
$$

