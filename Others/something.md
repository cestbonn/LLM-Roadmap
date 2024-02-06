# Something

When you want to study a particular topic, first check if there is a `survey` in that field. Next, research the famous works and topics of discussion in the field, starting with their papers. From the `introduction` section, you can learn about the background and challenges of the field. Then, take a look at the articles that are `cited`. This way, you can have a general understanding of the entire field.

## Use GPT
In fact, you can do this with GPT. This will give you a picture like a survey, however, to really understand techs and motivations, you should go check papers carefully.

- searching papers
- get intro and abstract part
- categorize papers in terms of `challenges` and `solution`
- take notes of `trick`

### Step 1. Searching

**Prompt 1**

>```
>Give me an develop tree of language model, where node is the model and mark the reasons of "why we need this mode compared with its father node", also, mark the core tech for handling that reasons. For example:
> 
>GloVe (Global Vectors for Word Representation) (2014)
>- From Word2Vec to GloVe
>- Reason: Word2Vec focuses on local context information, whereas GloVe aims to incorporate both global statistics and local context, providing a more comprehensive understanding of word meanings.
>- Core tech: Matrix factorization techniques on word co-occurrence matrices for word embeddings.
>```

**Prompt 2**

List models mentioned in the survey and ask GPT to build the tree.


### Step 2. Analyzing
**Why did the author propose the model?**
 1. What problem does this model aim to solve?
 2. List models from the same period.
 3. What are the drawbacks of those models?
 4. What drawbacks are overcome by the proposed model?
 5. To overcome those drawbacks, what contributions or innovations are proposed?
 6. To overcome those drawbacks, any advantages of those existing models are sacrificed?
 7. How is the proposed model evaluated?
 8. Despite the proposed model, what drawbacks or challenges still remain?
 9. What new drawbacks does the proposed model introduce?

**Details of model design**
1. what's the input and output of this model?

### Step 3. Comparing
Try the searching prompt1 with the context generated in Step 2.