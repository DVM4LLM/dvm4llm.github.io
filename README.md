



LLM agents have been demonstrated to be powerful in vision-language navigation (VLN) tasks. However, they often encounter challenges with sequential VLN tasks, particularly in adhering to instructions in prompts, which affects their overall efficacy. To unleash the efficacy of LLM agents against instruction violations, this paper proposes DVM, an LLM-based approach to self-identify and self-avoid instruction violations of any given LLM agent. Instead of altering the intrinsic mechanism of LLM agents, DVM operates externally on the input and output sequences of LLM agents. Specifically, DVM uses LLMs to decompose the instructions in the LLM agent's workflow into primitive constraints, creating oracles to detect any violations of these primitive constraints and synthesize avoiding actions to preempt potential violations. This paper also demonstrates the application of DVM to enhance three state-of-the-art LLM agents, assessing  DVM's effectiveness on two challenging VLN tasks: WebShop and MoTIF. Evaluation results show that DVM significantly boosts the performance of LLM agents across various agents and tasks.
Notably, DVM doubles the success rate of LLM-planner, a state-of-the-art LLM agent dedicated to sequential VLN, from 17.2% to 34.5% on the MoTIF dataset, showcasing its capability to adapt LLM planning more flexibly and effectively in sequential VLN scenarios.

## Instruction violations in VLN

![Figure 1: Instruction violations in UI space navigation.](Figures/fig1.png?raw=true "Figure 1: Instruction violations in LLM-based UI space navigation.")



However, these strategies, which depend on iterative prompts to refine LLM planning, face significant challenges due to *instruction violations*—situations where LLMs do not adhere to the instructions in their prompts.  Figure 1 presents examples of instruction violations in vision language planning.
In the first case, the LLM agent clicks on a white bed, disregarding the specific color instruction provided.
In the second case, despite being prompted to learn from past attempts and explore new actions, the LLM agent repetitively interacts with the same button, ignoring the task instruction.

Such violations pose two main problems for existing sequential VLP approaches. First,  instruction violations can derail the planning process, potentially causing the entire VLP task to fail.  Second, instruction violation greatly reduces planning efficiency by leading to the exploration of redundant or irrelevant states.Manually crafting rules to avoid such instruction violations can be ad-hoc, labor-intensive, and generally impossible for unforeseen instruction violations, contradicting the objective of developing versatile LLM agents capable of autonomously executing a wide array of tasks.





## Primitive Concepts for Instruction Violation Identification and Avoidance

![Alt text](Figures/fig2.png?raw=true "Figure 2: Primitive Concepts for Instruction Violation Detection and Avoidance.")



We utilize the robust framework of *primitive concepts* to address the challenge of self-identifying and self-avoiding instruction violations. This strategy is grounded in two key observations: the prevalence of primitive concepts in VLP tasks and the effective learning of these concepts by existing LLMs.



## Overview of DVM to improve a given LLM agent

![Figure 3](Figures/fig3_new.png?raw=true "Figure 3: Overview of DVM")



Figure 3 illustrates the workings of DVM, which operates externally to a given LLM agent, rather than modifying the agent's internal workflow. DVM functions by continuously assessing the outputs (i.e., the planned actions) from the LLM agent and adaptively modifying the inputs (i.e., the observations) provided to it. DVM utilizes LLMs to generate a set of oracles corresponding to individual primitive concepts outlined in the task instructions and the agent's reasoning process. DVM then evaluates whether the LLM agent's planned action conflicts with any of these oracles. In instances where a violation is detected, DVM removes any elements within the original observation that could potentially lead to a violation of the oracles.
Consequently, the LLM agent receives a *vetted* observation, free from elements that might cause instruction violations. Armed with this refined observation, the LLM agent is then able to re-plan its actions, ensuring compliance with the oracles and thus adhering to the task instructions more accurately. 

## Main Results

![Table 12](Figures/table12.png?raw=true "Main results on MoTIF and WebShop")

Table 1 and Table 2 showcase our main results, from which we have three observations.

First, DVM improves the efficacy of LLM agents consistently across different benchmarks and LLM agents.
On MoTIF, DVM improves the LLM-Planner's Success Rate (SR) from 17.2% to 34.5% (100.6% relative improvement), and the Completion Rate (CR) sees a 64.6% increase from 29.4% to 48.4% (64.6% relative improvement).  For ReAct, DVM improves the SR from 13.8% to 19.0%, a relative improvement of 37.7%, and lifts the CR from 25.5% to 28.0%, which is a 9.8% relative increase. Reflexion also benefits, with SR and CR rising by 16.7% and 20.9%, respectively. Turning to the WebShop results, DVM-enhanced agents again demonstrate superior performance.  ReAct sees its SR grow from 27.0% to 39.0% and its CR from 60.6% to 69.2%, reflecting relative improvements of 44.4% and 14.2%, respectively. Reflexion's SR escalates from 33.0% to 38.0%, a relative rise of 15.2%. The LLM-Planner experiences the most pronounced gains on this benchmark, with SR and CR soaring to 41.0% and 75.7%, from 30.0% and 58.6%, showcasing impressive relative enhancements of 36.7% and 29.2%, respectively.

Second, the general strategy of avoiding instruction violations tends to bolster the performance of LLM agents.  Reflexion, which incorporates self-reflective mechanisms, possibly sidesteps certain instruction violations through introspection of prior actions. This intrinsic capability results in a smaller observed benefit from DVM implementation. Yet, despite Reflexion's self-reflective strategies, it does not outpace DVM with the dedicated capability of avoiding instruction violations.  This advantage is further exemplified by DVM's capacity to enhance Reflexion's effectiveness, attesting to its general applicability.

Third, DVM shows particularly marked improvements in more sophisticated sequential VLN agents. The LLM-Planner, being specifically designed for sequential VLN tasks, features a high-level planner that breaks down the planning process into discrete, manageable steps. DVM's vigilant avoidance of action primitive concept violations significantly empowers the LLM-Planner, leading to the most substantial performance enhancements across both the WebShop and MoTIF datasets. This underscores DVM's vital role in realizing the full potential of advanced LLM agents within sequential VLN tasks.

## Statistical Analysis of DVM Process

<p align="center">   <img src="Figures/requested_exp.png?raw=true" alt="Statistical Justification of DVM Process" width="600"/>   </p>



### 1. Result Analysis

We statistically evaluate the DVM process using controlled experiments involving 100 instructions gathered from WebShop with 215 primitive concepts, each verified against manually confirmed ground truth labels. As shown in Table 3, DVM demonstrated high accuracy in concept extraction and oracle generation, aligning with the proficiency of LLMs in handling primitive concepts as discussed in our paper and evidenced by the results in Table 4 in our paper. In addition, Table 3 reveals that intuitive approaches to multiple rounds of vetting (e.g., selecting the optimal result from three attempts) offer minimal benefit, leading to our decision of not using this strategy in our work.

### 2.Failed Cases

DVM's effectiveness relies on LLMs to comprehend primitive concepts, a proficiency presumed in our Introduction(Line 100-103) and demonstrated by our results (Table 4).While DVM's process in Table 3.

The accuracy of oracle and avoiding action is still not perfect, so we examine the failed cases.

**Atomicity of Primitive Concepts.** On Webshop, the task instruction expects a good with color being gray. DVM identifies the color primitive concept and the constraint to be gray. When checking an item with its color being silver-gray, DVM treats the color satisfies the constraint and does not treat the item violates the color constraint. We investigate the LLM's understanding of the two colors as follows:

```
Prompt: 
Is silver-gray a gray color?

LLM Response: 

Yes, silver-gray is considered a variation of gray color. It is typically a lighter shade of gray that resembles silver, often with a slight metallic sheen or luster, akin to the color of polished silver metal. This color combines aspects of both silver and gray, resulting in a hue that is neutral and usually quite light, but with a touch of brightness often associated with metallic silver. Silver-gray can be used in various design contexts where a light, neutral, and slightly sophisticated color is desired.
```

Based on the LLM's understanding,  silver-gray is a variation of gray color, so the mistake results from the LLM understanding of primitive concepts. While the current number of failed cases is low, we believe DVM can benefit from interative correction of such misaligned commonsense knowledge especially when tackling instruction violations involving complex concepts less familiar to LLMs, which is a promising avenue for future exploration.



## Example Prompts for Concept Extraction & Oracle Generation

We give concrete examples for how to use LLM to extract primitive concepts from 

#### 1.1 Concept extraction from task instruction

**Task Instruction**: 

```
i'm looking for a high resolution digital film & photo scanner that is easy to use. choose the black ones that is 9.4 x 7.9 x 5.1 inch in size, and price lower than 180.00 dollars
```

**Prompt**:

```
I am using an LLM agent to perform a task on WebShop.
There are some requirements on specific aspects of the desired actions in task instructions.
Please extract the requirments, like"
Input : The task instruction is i am looking to buy a woman's us size 5 high heel shoe with a rubber soleand color patent-beige,and price lower than 90.00 dollars”.
Output : The requirements for the desired actions as specified in the task instructions are:
1 Product Type: Woman's high heel shoe
2.Size: US size 5
3.Sole Material: Rubber sole
4.Color: Patent-beige
5.Price: Lower than $90.00 dollars
These requirements should guide the search or filtering process in the WebShop to identify the correct item that matches all the given criteria.
"
Now, the Input: "the task instruction is i'm looking for a high resolution digital film & photo scanner that is easy to use. choose the black ones that is 9.4 x 7.9 x 5.1 inch in size, and price lower than 180.00 dollars".
Output:
```

**LLM Output (Extracted Concepts) :**

```
The requirements for the desired actions as specified in the task instructions are:

1. Product Type: High resolution digital film & photo scanner
2. Ease of Use: Easy to use
3. Color: Black
4. Size: 9.4 x 7.9 x 5.1 inches
5. Price: Lower than $180.00 dollars
```



#### 1.2 Concept extraction from LLM reasoning outputs.

State-of-the-art LLM agents use internal reasoning to improve the planning effectiveness in VLN tasks. These internal reasoning can contain useful instructions. We present an example of ReAct

**ReAct Internal Reasoning**: 

```
Action: think[The item has options '1 bottle + 60 capsules' and '16 fl oz (pack of 1)', and I need it in bottle for with 60 capsules, so I will click '1 bottle + 60 capsules'.]
```

**Prompt**:

```
I am using an LLM agent to perform a task on WebShop.
There are some requirements on specific aspects of the desired actions in task instructions.
Please extract the requirments, like"
Input : i will buy a woman's us size 5 high heel shoe with a rubber soleand color patent-beige,and price lower than 90.00 dollars”.
Output : The requirements for the desired actions as specified in the task instructions are:
1 Product Type: Woman's high heel shoe
2.Size: US size 5
3.Sole Material: Rubber sole
4.Color: Patent-beige
5.Price: Lowerthan $90.00 dollars
These requirements should guide the search or filtering process in the WebShop to identify the correct item that matches all the given criteria.
"
Now, the Input: "The item has options '1 bottle + 60 capsules' and '16 fl oz (pack of 1)', and I need it in bottle for with 60 capsules, so I will click '1 bottle + 60 capsules'."
Output:
```

**LLM Output (Extracted Concepts) :**

```
Based on the provided task instruction, the requirements for the desired actions are:

1.Form Option: 1 bottle + 60 capsules
```





### 1.2 Oracle Generation

**Prompt:**

```
For each requirement as input and give a program that checks whether a given observation violates the requirement as your output.
Example:
"Input :  For size constraint: Us size 5,generate a program that checks whether agiven observation violate the requirement.
Output : 
import re
def check size(description):
# Define the pattern to search for the size
# This pattern looks for the phrase "size" followed by a number, possibly with "us" preceding it
pattern = re.compile(r"b(us\s*)?size\s*5",re.IGNORECASE)
# Check if the description matches the pattern
if pattern.search(description):
    return False # The product does not violate the size constraint
else:
return True # The product violates the size constraint"

Input: For price constraint: Lower than $180.00 dollars

Output:
```



**LLM Output (Generated Oracle):** 

```
def check_price(price):
		if price < 180.00:
        return False  # The product does not violate the price constraint
    else:
        return True  # The product violates the price constraint
```



