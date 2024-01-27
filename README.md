# DVM: Self-Identifying and Self-Avoiding Instruction Violations of LLM Agents in Sequential Vision Language Planning

## Figure 1: Instruction violations in sequential VLP
![Alt text](Figures/fig1.png?raw=true "Figure 1")

## Figure 2: Primitive Concepts for Instruction Violation Identification and Avoidance
![Alt text](Figures/fig2.png?raw=true "Figure 2")

## Figure 3: Overview of DVM to improve a given LLM agent without changing its intrinsic logic
![Alt text](Figures/fig3.png?raw=true "Figure 3")



<table>
<thead>
  <tr>
    <th rowspan="2">Approaches</th>
    <th colspan="2">Original</th>
    <th colspan="2">with DVM</th>
  </tr>
  <tr>
    <th>SR</th>
    <th>CR</th>
    <th>SR</th>
    <th>CR</th>
  </tr>
</thead>
<tbody>
  <tr>
    <th colspan="5">Goal Instruction only</th>
  </tr>
  <tr>
    <th>Seq2Seq</th>
    <td>8.8</td>
    <td>19.8</td>
    <td>-</td>
    <td>-</td>
  </tr>
  <tr>
    <th>MOCA</th>
    <td>6.9</td>
    <td>21.2</td>
    <td>-</td>
    <td>-</td>
  </tr>
  <tr>
    <th>Seq2Act</th>
    <td>1.9</td>
    <td>5.4</td>
    <td>-</td>
    <td>-</td>
  </tr>
  <tr>
    <th colspan="5">LLM-based; Goal instruction only</th>
  </tr>
  <tr>
    <th>ReAct</th>
    <td>13.8</td>
    <td>25.5</td>
    <th>19.0</th>
    <th>28.0</th>
  </tr>
  <tr>
    <th>Reflexion</th>
    <td>19.0</td>
    <td>26.5</td>
    <th>22.4</th>
    <th>31.9</th>
  </tr>
  <tr>
    <th>LLM-Planner</th>
    <td>17.2</td>
    <td>29.4</td>
    <th>34.5</th>
    <th>48.4</th>
  </tr>
</tbody>
</table>


<table>
<thead>
  <tr>
    <th rowspan="2">Approaches</th>
    <th colspan="2">Original</th>
    <th colspan="2">with DVM</th>
  </tr>
  <tr>
    <th>SR</th>
    <th>CR</th>
    <th>SR</th>
    <th>CR</th>
  </tr>
</thead>
<tbody>
  <tr>
    <th colspan="5">Non-LLM-based</th>
  </tr>
  <tr>
    <th>IL</th>
    <td>29.1</td>
    <td>59.9</td>
    <td>-</td>
    <td>-</td>
  </tr>
  <tr>
    <th>IL+RL</th>
    <td>28.7</td>
    <td>62.4</td>
    <td>-</td>
    <td>-</td>
  </tr>
  <tr>
    <th>Human Expert</th>
    <td>59.6</td>
    <td>82.1</td>
    <td>-</td>
    <td>-</td>
  </tr>
  <tr>
    <th colspan="5">LLM-based</th>
  </tr>
  <tr>
    <th>ReAct</th>
    <td>27.0</td>
    <td>60.6</td>
    <th>39.0</th>
    <th>69.2</th>
  </tr>
  <tr>
    <th>Reflexion</th>
    <td>33.0</td>
    <th>66.5</th>
    <th>38.0</th>
    <td>65.3</td>
  </tr>
  <tr>
    <th>LLM-Planner</th>
    <td>30.0</td>
    <td>58.6</td>
    <th>41.0</th>
    <th>75.7</th>
  </tr>
</tbody>
</table>
