---
created: 2021-06-01T03:21:39 (UTC +08:00)
tags: []
source: https://law.mit.edu/pub/deconstructinglegaltext/release/1
author: Adam Nicholas
---

# Deconstructing Legal Text: Object-Oriented Design in Legal Adjudication · MIT Computational Law Report

> ## Excerpt
> The possibility of completing high fidelity machine translations is one that has captured the attention of linguists, mathematicians, and others throughout history. This project explores this possibility in the context of law through a combination of law, data science, and UI/UX.

---
Rules are pervasive in the law. In the context of computer engineering, the translation of legal text into algorithmic form is seemingly direct. In large part, the field of law may be ripe for expert systems and machine learning. For engineers, existing law appears formulaic and logically reducible to ‘if, then’ statements. The underlying assumption is that the legal language is both self-referential and universal. Moreover, descriptions are considered distinct from interpretations, i.e. in describing the law, the language is seen as quantitative and objectifiable. Nevertheless, is descriptive formal language purely dissociative? From the logic machine of the 1970s to the modern fervor for artificial intelligence (AI), governance by numbers is making a persuasive return. Could translation from words to numbers, in this context, be possible?

This project considers a fundamentally semantic conundrum: _what is the significance of ‘meaning’ in legal language?_ The project, therefore, tests the concept of translating laws by deconstructing sentences from existing legal judgments to their constituent factors. Definitions are then extracted in accordance with the interpretations of the judges. The intent is to build an expert system predicated on the alleged rules of legal reasoning. The authors apply both linguistic modelling and natural language processing technology to parse the legal judgments. The project extends beyond prior research in the area, combining a broadly statistical model of context with the relative precision of syntactic structure. The preliminary hypothesis is that, by analyzing the components of legal language with a variety of techniques, we can begin to translate law to numerical form.

Rules are pervasive in the law. In the context of computer engineering, the translation of legal text into algorithmic form is seemingly direct. In large part, the field of law may be ripe for expert systems and machine learning. For engineers, existing law appears formulaic and logically reducible to ‘if, then’ statements. The underlying assumption is that the legal language is both self-referential and universal. Moreover, descriptions are considered distinct from interpretations, i.e. in describing the law, the language is seen as quantitative and objectifiable. Nevertheless, is descriptive formal language purely dissociative? From the logic machine of the 1970s to the modern fervor for artificial intelligence (AI), governance by numbers is making a persuasive return. Could translation from words to numbers, in this context, be possible?

Recently, Douglas Hofstadter commented on the “Shallowness of Google Translate.”1 He referred largely to the Chinese Room Argument: that machine translation, while comprehensive, lacked the capacity of understanding.2 Perhaps he probed at a more important question: does translation _require_ understanding? Hofstadter’s experiments indeed seemed to prove it so. He argued that the purpose of language was not about the processing of texts. Instead, translation required imagining and remembering “a lifetime of experience and… of using words in a meaningful way, to realize how devoid of content all the words thrown onto the screen by Google translate are.”3 Hofstadter comments that while Google translate had the appearance of understanding language, the software was merely ‘bypassing or circumventing’ the act.4  

Yulia Frumer, a historian of science, notes that translation not only requires producing adequate language of foreign ideas, but also the “situating of those ideas in a different conceptual world.”5 That is, with languages belonging to the same semantic field, the conceptual transfer in the translation process is assumed. However, with languages that do not share similar intellectual legacies, the meaning of words must be articulated through the conceptual world in which the language is seated.

Frumer uses the example of 18th century Japanese translations of Dutch scientific texts. The translation process involved first analogizing from Western to Chinese natural philosophy, effectively reconfiguring foreign concepts into local concepts through experiential learning. This is particularly fascinating, provided that scientific knowledge inherits the reputation of universality. Yet, Frumer notes, “if we attach meanings to statements by abstracting previous experience, we must acquire new experiences in order to make space for new interpretations.”6  

Mireille Hildebrandt teases this premise by addressing the inherent challenge of translation in the computer ‘code-ification’ process. Pairing speech-act theory with the mathematical theory of information, she investigates the performativity of the law when applied to computing systems. In her synthesis of these theories, she dwells on the concept of meaning. “Meaning,” she states, “depends on the curious entanglement of self-reflection, rational discourse and emotional awareness that hinges on the opacity of our dynamic and large inaccessible unconscious. Data, code… do not attribute meaning.”7 The inability of computing systems to process meaning raises challenges for legal practitioners and scholars. Hildebrandt suggests that the shift to computation necessitates another shift: from reasoning to statistics. Learning to “speak the language” of statistics and machine-learning algorithms would become important in the reasoning and understanding of biases inherent in legal technologies.8  

More importantly, the migration from descriptive natural language to numerical representation risks discarding critical context as ideas are (literally) ‘lost in translation.’ Legal concepts must necessarily be reconceptualized for meaning to exist in mathematical form. The closest analogue in semantic ancestry would be legal formalism. Legal formalists interpret law as rationally determinate. Judgments are deduced from logical premises and meaning is assigned. While, arguably, the formalization of law occurs ‘naturally’ – as cases with similar factual circumstances often form rules, principles, and axioms for legal treatment – the act of conceptualizing the law as binary and static is confounding. Could the law behave like mathematics, and thereby could the rule of law be understood as numeric?

Technology not only requires rules be defined, but that such rules are derived to achieve specified outcomes. Currently, even with rules that define certain end-states, particularized judgments remain accessible. Machines, on the other hand, are built on logic, fixed such that the execution of tasks becomes automatic. Outcomes are characterized by their reproductive accuracy. Judgments, on the other hand, are rarely defined by a measure of accuracy. Instead, they are weighed against social consensus.

To translate the rule of law into mathematical notation would require a reconfiguration of legal concepts. Interestingly, the use of statistics and so-called ‘mathematisation’ of law is not novel. Oliver Wendell Holmes Jr. most famously stated in the _Path of Law_ that “\[f\]or the rational study of the law, the blackletter man may be the man of the present, but the man of the future is the man of statistics and the master of economics.”9 Governance by numbers then realizes the desire for determinacy; the optimization of law to its final state: stability, predictability, and accuracy. The use of formal logic for governance has a rich ancestry with a common theme: that mathematical precision should be applied across all disciplines.

Since the twelfth century, mathematical logicians have used logical paradoxes to spot ‘false’ arguments in courts of law.10 In the seventeenth century, Gottfried Leibniz proposed a mental alphabet,11 whereby thoughts could be represented as combinations of symbols, and reasoning could be performed using statistical analysis. From Leibniz, George Boole’s infamous treatise, _The Laws of Thought_,12 argued that algebra was a symbolic language capable of expression and construction of argument. By the end of the twentieth century, mathematical equations were conceivably dialogic – a form of discourse.

This was perceivably owed to Boole’s perception that complex thought could be reduced to solutions of equations. Nevertheless, the most fundamental contribution of Boole’s work was the capacity to isolate notation from meaning.13 That is, ‘complexities’ of the world would fall into the background as pure abstraction is brought into focus. Eventually, Boole’s work would form the basis of the modern-day algorithm and expression in formal language.

With the rise of artificial legal intelligence, computable law is making a powerful return. Legal texts may be represented as computational data with terms made ‘machine-readable’ through a process of conversion. Despite the capacity to express legal language in alternative computable forms, the notion of interpretation appears to have changed. How would digital data inscription and processing alter methods of legal reasoning?

## _a. Research Question and Outline of Approach_

This project considers a fundamentally semantic conundrum: what is the significance of ‘meaning’ in legal language? From a statistics standpoint, meaning can be approximated. By applying word analogies as the ‘mathematical’ basis, meaning is gauged by the statistical probability of a particular response. In recognizing the context and relationship between words, meaning hinges on the frequency of its appearance in a particular arrangement. That is, what do its neighbors reveal about the word in question?

Reflecting on Hildebrandt and Frumer, meaning is associated with experience. Therefore, finding the meaning of legal concepts would require abstracting from experience. Should experience be built from distinct conceptual worlds, to move across these worlds would be to translate. Translating legal language then requires a reframing of legal concepts, perhaps an expression of the law based on statistical experience as opposed to natural language.

The project will proceed in two phases: (1) the proof of concept (POC); and (2) application to broader legal corpora. In the first phase, the POC will analyze three United States Supreme Court cases. The selection was chosen on the basis of a similar factual premise and historical proximity. That is, all three cases involve defining the use of firearms and were ruled in rapid succession. These cases are _Smith v. United States_ (1993), _Bailey v. United States_ (1995), and _Muscarello v. United States_ (1998). While there are evidently a number of caveats14 to this selection, it nonetheless has merit as an interesting starting point. Notably, the POC wrestles with the existence of legal concepts. The goals of the POC are two-fold: (1) to analyze the processes involved with legal interpretation and reasoning; and (2) to critically assess them against the function of the law.

Methodologically, the POC tests translation by deconstructing sentences from existing legal judgments into their constituent factors. Definitions are then extracted in accordance with the interpretations of the judges. The intent is to build an expert system predicated on alleged rules of legal reasoning. The authors intend to apply both linguistic modelling and natural language processing (NLP) technology to parse the legal judgments. The preliminary hypothesis is that, by analyzing the components of legal language using a variety of techniques, we can begin to translate the law into numerical form. Furthermore, it would be interesting to consider what contextual understanding would be necessary to understand the language of various legal documents.

Following the POC, the authors will extend the scope of translation to the corpora of United States Supreme Court cases. This stage of the research will also consider the feasibility of expanding the approach to similar legal texts. For the purposes of the current paper, the authors will focus on the observations and findings from the POC. The authors believe that the initial assessment at the POC stage could contribute to a more fruitful dialogue on the integration of computational technology in law.

The POC complements existing literature on Law2Vec and legal word embeddings. Equally, the project extends beyond prior research in the area, combining a broadly statistical model of context with the relative precision of syntactic structures. In effect, the POC intends to generate building blocks to determine “context” contained in the text, allowing us to define the use of firearms through a framework of extraction.

The paper will proceed as follows. Part I will begin with a literature review of texts that have informed the project’s inquiries and the environment which it intends to resolve. As the nature of the paper is fundamentally interdisciplinary, it draws reference from law, linguistics, and computer science. Part II discusses the methodology the authors have taken; highlighting both the elements of inspiration and the strategies considered. Part III teases at preliminary observations and notes of interest during the project’s progression. Part IV details the technological implementation and the actual steps taken to generate the translation. Part V reflects on early achievements and areas of further advancement. The authors will then conclude with a review and next steps.

## _a. Jurisprudential Heritage_

AI adjudication is an evidently polarized subject. Questions around the prospect of “robot judges” typically center on morality and equitable justice,15 on issues of explainability and Black Box machine learning.16 In common law systems, the art of drafting legal opinions begins with mastering legal argumentation. To ground the argument within the sphere of existing legal texts is the linchpin of judicial decisions.

Legal theory becomes a reference point when courts are asked to interpret legal documents. Textualism, for example, “narrow\[s\] the range of acceptable judicial decision-making and acceptable argumentation”17 by turning to dictionary definitions and rejecting judicial speculation. Yet, what is the purpose of ‘narrowing the range’? To that question, Antonin Scalia answers, “…textualism will provide greater certainty in the law, and hence greater predictability…”18 So, what are its assumptions and implications? Eric Posner suggests there may be aspirational intentions “to keep the law pure”19 or, otherwise, to ensure that the legal system is consistent. Textualism also reinforces the role of judges. Within the concept of textualism, the role of the judge is to passively interpret the law, with only semantic legal interpretations.20  

Consider the infamous example of a municipal legislation stating that “no person may bring a vehicle into the park.”21 Would an ambulance be permitted to enter the park in the event of an accident? For textualists, they may argue that, according to the dictionary definition, an ambulance is a vehicle, which therefore, cannot enter the park. Should the legislators have thought an ambulance was an exception, they would have included it in the text. Accepting the premise of that argument, what about a police car or a firetruck? Perhaps the legislation should be amended to include all emergency vehicles. What happens then if an ambulance is merely parked inside the park with no foreseeable emergency?

The example illustrates the problem with textualism: the doctrine becomes rapidly cyclical, as interpretations rendered must either become increasingly narrow or increasingly broad to accommodate a “myriad \[of\] hypothetical scenarios and provide for all of them explicitly.”22 Textualism, therefore, falls down the slippery slope of literalism. Words of legal texts are assumed to embody intrinsic meaning and are waiting to be extracted.

Moreover, the process of mere ‘extraction’ has precedential value. The approach, taken most prominently in common law systems, is to follow past decisions. Adopting the decisions of the past to guide future conduct parallels this exact act of extraction. That is, applying past precedents provides the scope for a “gradual moulding of the rules to meet fresh situations as they arise.”23 Decisions have binding legal force. Interpretations of the past should carry the definitions to be used moving forward. With this perspective, the role of the judge is that of an archaeologist, excavating legal truths from the judicial past.

This is seemingly straightforward. Yet, identifying within the decision the kernel of precedent remains challenging. Holmes describes the challenge as a paradox of form and substance in the development of the law. The form is logical, as “each new decision follows syllogistically from existing precedents.”24 Still, its substance is legislative and draws on views of public policy. Holmes argues that the law is driven by the “unconscious result of instinctive preferences and inarticulate convictions;” and therefore, “the law \[is\] always approaching, and never reaching, consistency.”25 The ostracized conclusion would be that judicial decisions necessarily have an element of inexplicability, and are, in fact, a ‘Black Box.’26 Recalling Hildebrandt, “meaning” then becomes a metaphor and the heart of the juridical process.

The significance of the paper is, in part, to unpack the paradox articulated by Holmes. The selected cases aim to paint a picture on the use of precedent as a legal tool, and to consider whether the law subconsciously follows a logic. To create the painting, the authors draw inspiration from the field of linguistics.

## _b. Linguistic Influence_

Understanding the underlying hierarchical structure of a language is key to breaking down its sentences in a meaningful manner. Analyses of sentence structure fall primarily into two schools of thought: (1) dependency; and (2) phrase structure. The former, commonly represented as dependency trees, begins with the root verb of the superordinate clause and branches out from there, with subordinate verbs arranging substructures. Dependency trees map one node to each word without projecting constituent phrases: each word simply depends on another. For example, in most English sentences, the subject typically falls to the left of the verb, while its other dependencies (e.g., its objects) fall to the right. Since each word in a dependency syntax is represented by precisely one node, structural redundancy is arguably decreased. This system has been characterized as well-suited for algorithmic translation from natural language, owing to the node conservatism and predictability of anchoring sentences through its verbs.

Alternatively, phrase-structure representations, notably spearheaded by Noam Chomsky,27 use constituency relations. In contrast with dependency trees, each ‘constituent’ (or, individual element) in a sentence is headed by its own phrasal node. Subsequently, purely binary branching can occur. The elegance of these representations is that they work generatively. That is, even a small selection of rules can produce a wide variety of structures found across natural languages. Furthermore, constituency embraces analysis of underlying structure and transformations, accounting for numerous phenomena such as subject-verb inversion in interrogatives.28 Phrase-structure representations also permit a powerful structured analysis of syntactic relationships.29  

Semantic form traditionally involves the classical theory of concepts, otherwise known as definitionism or componential analysis. Here, semantic meaning is encapsulated as a combinatorial set of true/false statements, akin to a checklist of conditions. For example, _apple_ might be composed of _+fruit, +green_, and _+round_. Classical theory, therefore, considers the componential elements from which semantic meaning is formed, allowing for a systematic view on word-to-word relationships and validity.30  

However, classical theory is often criticized for its failure to account for phenomena such as the subjectivity or typicality of definitions.31 Ludwig Wittgenstein posited, through his analogy with ‘family resemblance,’ an underlying prototype theory of concepts, as opposed to a fixed set of composite definitions. The theory claims that some concepts are regarded as more ‘typical’ of a category than others. For example, a robin is a more prototypical bird than an emu or a penguin. Consequently, these observations must be factored into the linguistic system.32  

What further complicates the matter is the incongruence between semantics and pragmatics: the former concerns language independent of real-world context, whereas the latter is hinged upon situational context. Essentially, pragmatics is the application of semantics within context.33 Consider the phrase, “it’s rather chilly in here.” Semantically, the meaning of the phrase is perhaps that, according to the speaker, “there is a place X in which the temperature is lower than is comfortable.” Given additional knowledge that the phrase was taken from a dialogue between two individuals, the phrase pragmatically could mean “please close the window for me;”34 therefore, the reason for the choice of phrasing is likely owed to courtesy. More importantly, this form of expression is indicative of the flexibility of language and its inseparability from context: context necessarily contributes to meaning.

While semantics concerns the inherent and invariant properties of words and their combinations, pragmatics progresses into the realm of context and implicatures. Consequently, pragmatics in the context of NLP is seen as problematic: expert systems do not have the ability to infer extended meaning from context. Interestingly, legal texts are often regarded as rather structural, perhaps even devoid of pragmatic content. Given the aforementioned premise, is legal language anchored exclusively in semantics? If so, how amenable is legal language to NLP analysis?

## _c. Technological Staging: AI and Law_

Evidently, inspiration from linguistics is not novel as “law has language at its core.”35 Christopher Markou and Simon Deakin point to breakthroughs in NLP that have contributed to the emergence of ‘Legal Technology.’ They identify the pressure points at which computability falls short: where the legal system is incompatible with computer science.

In fact, they cleverly evoke Chomskyan and rationalist approaches to designing “hard-coded rules for capturing human knowledge.”36 Chomsky’s work stirred further developments in NLP, eventually powering advances in machine translation and speech recognition. These advances, undoubtedly, were enabled by Deep Learning37 models that were able to abstract and build representations of human language. Despite the significant leaps brought on by such technologies, the threat discussed by Markou and Deakin endured, stemming from an underlying anxiety against “the epistemic and practical viability of using AI and Big Data to replicate core aspects and processes of the legal system.”38  

Subsequently, their reimagining of a legal system – one predicated on a hyper-formalized method of reasoning39 – warns of the conceivable incongruence with the current normative legal structure. Using employment status as a test case, their paper first explores similarities between legal processes and machine learning technology. Markou and Deakin note two key parallels: (1) abstraction to conceptual categories, and (2) error correction and dynamic adjustment.

Nevertheless, their thesis, or claim of divergent paths, is the quality of reflexivity40 in legal knowledge. That is, legal categories both shape and are shaped by the “social forms to which they relate.”41 In other words, the existence of such categories is dependent on the force of law,42 and there is continual reference between the law and its socially complex environment. The law cannot be divorced from its societal embedding. As a result, the law could never be descriptive, but is rather ‘naturally’ prescriptive. Markou and Deakin, therefore, identify a fundamental philosophical mismatch as opposed to a structural, process-oriented incongruity. Their conclusions identify legal reasoning as extending beyond the straightforward application of rules to facts. Adjudication, for instance, is a means of “resolving political issues.”43 For Markou and Deakin, there is no exact science to judicial decisions “because of the unavoidable incompleteness of rules in the face of social complexity.”44 Judgments could only ‘approximate’ from historical precedent. Translation of legal categories into mathematical function is, thus, not possible since the flexibility and contestability of natural language cannot be completely captured by algorithm.45  

Holmes’s paradox resurfaces. Holmes notes that to “attempt to deduce the corpus from _a priori_ postulates, or fall into the humbler error of supposing the science of the law reside\[s\] in the _elegantia juris_, or logical cohesion of part with part”46 mistakenly interprets law as systemically formalistic. While the issues identified by Markou and Deakin are undeniably significant, their arguments rely on the premise of a complete substitution of legal systems. That is, they warn of a project to replace entirely juridical reasoning with machine learning. Accordingly, there are sweeping inferences on the incompatibility of AI and law, bringing to light only one side of Holmes’s paradox: the law is syllogistic in form.

Yet, there may be merit to an analysis at a micro-level. Programming languages may be able to perform many of the demands called upon for the functioning of society. Acknowledging that language both constitutes law and is capable of realizing foundational rule of law principles, we again reassess the translation of natural language to computer code. The law hinges on complicated social and political relationships,47 and more importantly, metaphors that require latent understanding of temporal societal constructs.48 This suggests there may be a space to regard AI as complementary,49 rather than substitutive, of legal actors. The key is to employ the proper language game.50  

For Lawrence Lessig, the conceptualization of code as law is not novel. Instead, he has drawn attention to code as a form of control in ‘cyberspace,’ that “code writers are increasingly lawmakers.”51 The difficulty, of course, is defining the parameters of cyberspace. In this case, cyberspace may refer to the entirety of the legal sphere. To return to Markou and Deakin, their arguments repeatedly point to the model of ‘legal singularity.’52 ‘Legal singularity’ draws from an association of the law as precise, predictable, and certain in its function.53 The complexity of developments in machine learning for law suggests that legal singularity could be achievable.

In a vibrant thought experiment, Casey and Niblett suggest that existing legal forms will become irrelevant as machines enable the development of a new type of law: the micro-directive. The micro-directive is conceptually a new linguistic form, offering “clear instruction to a citizen on how to comply with the law.”54 In this futuristic construct, lawmakers would only be required to set general policy objectives. Machines would bear the responsibility to examine its application in all possible contexts, creating a depository of legal rules that best achieve such an objective. The legal rules generated would then be converted into micro-directives that subsequently regulate how actors should comply with the law.

Imagining the legal order as a system of micro-directives, the law finds itself drawn to a linguistic structuralist framework, carrying forth the jurisprudential work of Kelsen and the “pure science of law.”55 Just as a norm expresses not what is, but what _ought_ to be – given certain conditions – the micro-directive draws attention to the semiotics of legal argument. Like Kelsen’s norms, the micro-directive rests on the principle of effectiveness. The legal order relies on the assumption of being efficacious, such that its citizens conduct themselves in pure conformity with it.56 But, on what principle? The micro-directive rests on a ‘law and economics’57 framework of effectiveness. Seated within the technical authority of AI,58 the micro-directive distorts the realities of legal reasoning by removing value judgments from the adjudication process. The presumption that machines are able to generate neutral sets of information, then translate such information into perfectly comprehensible instruction, is evidently misinformed. It stands on the premise that translation operates without interpretation. More importantly, the presumption strategically excludes the actors involved in the translation, inadvertently conferring the _rule_ of law to code. The process of transforming a general standard to a micro-directive is, therefore, a process of subverting politics in its linguistic casing.

So, how then could code become the vehicle that shapes the law? In practice, the most obvious example is traffic laws and speeding regulations. Traffic lights “communicate the content of a law to drivers at little cost and with great effect.”59 The traffic light is regarded as translating legal complexity into a simple command. Traffic lights are increasingly being equipped with algorithmic technology to reflect real-time traffic flows and, accordingly, adjust the timing of light changes.60 Moreover, traffic lights may soon include sensors that could appropriately identify patterns of distress and types of vehicles to allow for expedited changes in the event of emergencies.

For Casey and Niblett, predictive models provide the content of the law. Micro-directives would then communicate the legal treatment of the particular conundrum.61 Legal actors would equally rely on such models to assess the acceptable plans of action for a particular diagnosis or factual circumstance. The micro-directive then reinvents the legal system, as legal language is eradicated and adopts a different linguistic form.

Though at polar ends of the spectrum, both Markou and Deakin and Casey and Niblett depend on the same underlying assumption of a wholesale replacement of legal reasoning. This approach certainly raises significant metaphorical eyebrows on the broad impacts of AI in law. However, this approach also avoids the nuances of the law that demand further analysis: in particular, the act of translation. Holmes described the “single germ multiplying and branching into products as different from each other as the flower from the root.”62 Thus, to make sense of the consequences of computational technology in law necessitates not an evaluation of the flower or the root, but the single germ.

Precision has often been argued as an essential component of legal language. Nonetheless, new factual circumstances create room for interpretation. How then could ‘code-ification’ occur to account for an ever-adaptive, and evolutionary, system? In the following section, the authors will outline the computational tools used in the translation process. More importantly, the authors will peel back the curtain behind translation; specifically, the decisions taken in the parsing of the legal judgments.

Prior literature on Deep Learning in legal text analytics traditionally discussed crafting knowledge bases to capture legal concepts and terminology.63 Ilias Chalkidis and Dimitrios Kampas reflect on existing techniques, but contribute further by building word embeddings64 trained over a large corpora of legal documents composed of legislation from the UK, EU, Canada, Australia, USA, and others.65 Applying the Word2Vec model,66 Chalkidis and Kampas’s own model – aptly named Law2Vec – offer a pre-trained set of legal word embeddings. Broadly, the process involves translating legal text into numeric form in order to calculate the relationships between legal terms. The calculation represents the probabilistic likelihood of one term appearing synonymous in the presence of the other. The main assumption is that “similar words tend to co-occur in similar contexts.”67  

Below is a table of a selected 20 words and their associated terms identified by the model:

![](https://resize-v3.pubpub.org/eyJidWNrZXQiOiJhc3NldHMucHVicHViLm9yZyIsImtleSI6Ing3MXB6bmFlLzQxNjA1NjIyNjIyMzE2LnBuZyIsImVkaXRzIjp7InJlc2l6ZSI6eyJ3aWR0aCI6ODAwLCJmaXQiOiJpbnNpZGUiLCJ3aXRob3V0RW5sYXJnZW1lbnQiOnRydWV9fX0=)

**Table 1:** Sample Legal Word Embeddings; _see_ Chalkidis and Kampas, _supra_ 65 at 176).

This is undoubtedly remarkable. The associations made between the identified legal terms are indicative of the competence of machine learning algorithms to analyze complicated legal texts. Most fascinating perhaps are the terms associated with the word ‘immigrant’ found by the algorithm. Beyond locating synonyms, the terms deemed as similar reveal the latent politics of labelling that have classified immigrants as akin to aliens. Nevertheless, Chalkidis and Kampas offer only a limited perspective on legal concepts. The terms marked as ‘legal’ provide a scope of the law that does not consider the inherent interpretative exercise performed in adjudication. The act of legal reasoning itself is not represented in their model. While Chalkidis and Kampas tease at the possibility of translation, their method is rather limited to word associations. Chalkidis and Kampas could only bring to light the calculated similarities between legal terms, but they do not unpack _how_ the similarity came about. In other words, the underlying process of deriving meaning is never exposed.

Moreover, the selection of terms deemed ‘legal’ is rather shallow. They are suggestive of a legal vocabulary, but do not probe into the function of these words. The authors, therefore, take inspiration from literature outside of the legal realm, focusing on the mechanics of linguistic reasoning and the adjudicative process.

## _a. Technical Inspiration_

As an introductory note on method, Markou and Deakin have helpfully outlined NLP technologies that have laid the foundation of current applications of AI-based innovations.68 NLP is a combined scientific and engineering exercise, applying “cognitive dimensions of…natural language” to “practical applications…\[of\] interactions between computer and human languages.”69 For the intentions of this paper, our focus will be on natural language in written form, i.e. text. Not only is written text the form in which law most typically resides, it is also the observable component of language that exists in symbolic form.70 Interestingly, mathematics – or to recall, the mental alphabets of Leibniz and Boole – is described as _the_ symbolic language.71 It follows that translation is most feasibly comparable where both ‘languages’ are in a similar state.

In order for natural language text to be ‘primed’ for translation, the authors applied an approach first introduced in the sphere of bioinformatics. In 2006, Fundel et al. developed RelEx, or the relation extraction of free text, to better understand the interactions between genes and proteins marked by existing biomedical publications. RelEx relies on natural language _preprocessing_, “producing dependency parse trees and applying a small number of simple rules to these trees.”72 RelEx extracts qualified relations from natural language text by first breaking down sentences into component words (tokens), and then uses a parser73 to create syntactic dependency trees. These dependency trees are then leveraged from group tokens into ‘noun-phrase’ chunks.74 The approach identifies qualified relations based on rules applied to dependency trees and their original sentences, which are then subjected to ‘filtering.’75 These rules would connect known proteins that interact with one another.

The approach used in the RelEx paper will be applied to the current analysis of legal judgments. In addition to noun-phrases, sentences will be deconstructed into the basic semantic building blocks of the English language,76 otherwise known as subject-verb-object (SVO) triplets. Sentences selected from each judgment are chosen based on their significance to the outcome of the judicial decision. These sentences are subsequently scanned for the presence of SVO triplets. Markers to each individual sentence based on equivalency, in order to then form connections between phrases.

Referring back to the aforementioned linguistic models, applying the RelEx method necessarily depends on a preference for dependency syntax and the classical theory of concepts (definitionism). Nevertheless, the authors argue that the mapping of each SVO component in reference to its neighboring components helps compensate the challenges involved with the multiplex nuances of word usage. By working with context, the analysis will extend beyond the realm of prototype theory,77 which struggles to explain properties arising from context and pragmatic inference.78 The graphing of the SVO triplets acknowledges context,79 thereby becoming an integral part of the overall analysis. This method overlaps with ideas addressed in cognitive linguistics, such as the theory-theory of concepts, that heavily relies on role and context. Furthermore, employing sets of meta-concepts, along with graphed contextual relations, provides an analogy of traversing the semantic and pragmatic layers of language.

This project is, therefore, guided by three key tools: (1) Python; (2) spaCy; and (3) Neo4j. The first is the formal scripting language used to write the translation algorithm. Python was chosen for its known flexibility and general use.80 Python also adapts in a number of design spaces, namely for tasks that are structural and reflective. spaCy is the chosen open source81 library for NLP. spaCy is the primary software used to help parse sentences from legal texts to dependency trees, allowing components to be organized into categories.

The decision to use spaCy, as opposed to other NLP packages available in Python, centered on its ease of use, configurability, speed, and existing models pre-trained on a generalized data (e.g. articles, comments, blogs, etc.).82 While intuitively NLP programs, such as LexNLP, were considered, the current test case poses a different challenge. LexNLP, for example, works with legal texts that are rather structured (i.e. contracts).83 Therefore, LexNLP is trained at the document and clause level, making it capable of extracting and classifying clauses as opposed to semantic content. The authors acknowledge that there are certainly merits to LexNLP, with its greatest advantage being models pre-trained on U.S. legal texts. Nevertheless, spaCy offers much more functionality and flexibility given the breadth of subject matter found in the training data. By way of analogy, the choice may be akin to choosing between an oyster knife and a Swiss army knife when asked to descale a bass. The oyster knife is specialized but has its practical limits. In contrast, the Swiss army knife – emblematic of versatility – may offer more options and space for creativity when handling intricate tasks.

Finally, Neo4j is a graph database management system designed to store and process data in the form of nodes and relations.84 The system helps classify the entities and the semantically relevant connections between such entities. Graph databases are commonly used for intermediate representation (IR). Known as the “stepping stone from what the programmer wrote to what the machine understands,”85 IR is an object-oriented structure that, in its final form, stores all information required to execute a specified program.86 IRs facilitate translations from natural language to machine code, bridging semantic gaps and behaving as the ‘middleman’ between syntactic forms. The graph database is also ideal for modelling dependency trees and object-oriented phenomena, such as inheritance. Put together, the authors attempt to advance the techniques inspired by RelEx for the translation of legal language into numeric form.

## _b. Risky Business: Case Selection_

The initial test cases selected for the POC are not arbitrary. The authors have strategically chosen cases that all follow a similar premise: what is the meaning of “use” when applied to a firearm? Importantly, the cases belong to an alleged lineage, demonstrating the application of precedent and consistency in legal adjudication.

In 1993, the Supreme Court of the United States was asked to rule on the definition of “use” in _Smith v. United States_. The petitioner, John Angus Smith, had offered to trade his gun in exchange for cocaine. He was subsequently charged with numerous firearm and drug trafficking offenses. These charges included “using” a firearm “during and in relation to” a drug trafficking crime, as stipulated under statute 18 U.S.C. §924(c)(1).87 The Court held that the trading of a firearm constitutes “use” within the meaning of the statute. There are two remarkable notes to this case. First, the Court interprets the meaning of use rather broadly, particularly applying emphasis on the “everyday meaning and dictionary definitions” of use. Second, the interpretation is placed within the limited context of drug trafficking. The Court shifts away from a dictionary definition and, instead, emphasizes the furtherance of a crime as necessary to establish “use” of a firearm.

In 1995, the Court was again asked to rule on the definition of use in _Bailey v. United States_. Similarly, the petitioners, Bailey and Robinson, were each convicted of drug offenses and of violating, none other than, 18 U.S.C. §924(c)(1).88 The factual difference is the state of the firearm “during and in relation to” the drug-related offense. The Court was, therefore, asked to determine whether accessibility and proximity to the firearm was indicative of use. The Court held that the statute required “evidence sufficient to show an _active employment_ of the firearm by the defendant, a use that makes the firearm an operative factor in relation to the predicate offense.”89 In _Bailey_, the Court then narrows the definition of use by including the element of “active employment.” The Court provides a justification for its decision by referring to Smith and noting the ordinary definition of “use” in the active sense is “to avail oneself of.”90 Strikingly, the act of bartering falls within active employment, even though the gun was exchanged passively.

Coincidentally, a third case – three years later – had arisen, requesting the Court to rule on the definition of use under statute 18 U.S.C.§924(c)(1). However, _Muscarello v. United States_ stretched beyond use and, instead, focused on “carries.”91 In _Muscarello_, enforcement officers had found guns in the petitioners’ vehicles stored in a locked glove compartment and trunk respectively. The Court was, therefore, asked to determine whether that sufficiently fell within the definition of “carries.” The Court ruled that carrying a firearm, in accordance with 18 U.S.C. §924(c)(1), “applies to a person who knowingly possesses and conveys firearms in a vehicle.”92 The Court again invokes “ordinary English,” otherwise, basic meaning in dictionaries, to argue that carry is synonymous with conveys. Moreover, the Court again refers to _Smith_, but unlike _Bailey_, directs its reasoning to the _purpose_ of the statute.93 Notably, in all three cases, ordinary meaning was put forth as a dominant basis for argumentation. Yet, the argument was always supplanted by intentions of Congress and the statute: that the purpose is to combat the “dangerous combination” of “drugs and guns.”94  

Perhaps to avoid a fourth case, Congress amended 18 U.S.C. §924(c)(1) to include “possess” in tandem with the phrase “in furtherance of any such crime,” thereby accommodating the outcomes rendered in _Smith_, _Bailey_, and _Muscarello_. This limited subsequent cases from arriving at the Court.95 These cases were, therefore, carefully selected to illustrate that judicial decisions could bear the epistemic flavors of textualism with an underlying subtext of policy. Moreover, their similarity in factual circumstances allow for a stronger test of the underlying mechanisms of judicial reasoning and legal argumentation.

Again, the cases selected are not without limitations. In fact, they were carefully chosen to better demonstrate the subtleties of language and linguistics in law. Equally, the authors acknowledge that there are shortcomings to the project; namely, the importance of fact in law. Geoffrey Samuel states, “law arises out of fact.”96 That is, the legal effect of precedent extends so long as the material facts of the case are analogous. The project, however, does not account for the facts of the cases. Instead, it focuses on the Court’s specific arguments on the meaning of “use,” accepting the facts as only peripheral to the exercise. The exclusion of facts may be problematic, given their significance to the nature of the common law system.97 Still, the intentions of the paper are not to replicate judicial reasoning in common law. Fundamentally, the focus of the POC is translation, specifically an attempt to operationalize the migration of legal texts in natural language to algorithmic form.

The inherent nature of interdisciplinary projects exposes the gaps between untraversed worlds. Between a data scientist, mathematician, linguist, and jurist, there are primarily two modes of operation. One is derived from logic, and the other in humanities. Moreover, the disciplines speak different technical languages. Indubitably, clashes arise. Yet, the unifying mission to uncover ‘meaning’ has raised interesting perspectives on method and interpretation.

Consider the conversation between the linguist and computer scientist. The linguist struggles with a possible SVO markup for open clausal complements. The computer scientist suggests that it would fit ‘cleanly’ in the code if this were marked in the same manner as a clausal subject. The linguist is bewildered. In dependency linguistics, an open clausal complement is a clause without a subject. A clausal subject, on the other hand, is when a whole clause is itself a subject. What might be problematic with this type of equivalency?

This particular concern was contemplated within the framework of ‘nested SVOs.’ Complex sentences are composed of several clauses that carry condition and inherence. For example: adverbial phrases or subordinate clauses, that are themselves SVOs, act as modifiers to an overarching (superordinate) SVO. This became problematic when resolving the SVOs with one another, threatening a possible misalignment between their semantic and syntactic representation.

Another fascinating example came about when assessing the difference between the following two sentences:98  

> “He shot the man with a gun.”
> 
> “He shot the man with a telescope.”

For the human mind, the role of the object evidently differs between the sentences. In the former, the gun is indicative of the weapon used by the perpetrator. In the latter, the telescope is a qualifier of the victim, drawing a sharper image for the reader. This is owed to the cognitive association99 between the object to the verb “shoot.” But, what happens should the gun qualify “the man” in the first sentence? If so, not only does it change the meaning of the sentence, but, more importantly, it could affect the ultimate charge against the perpetrator. That is, the crime could be charged as murder, manslaughter, or self-defense. The sentence alone cannot provide this depth of information required. Context and factual circumstances of the event are needed to determine how the sentence should be interpreted.

Interestingly, the data scientist and/or mathematician would approach the question by calculating the cosine similarity between the vector representations (word embeddings) of the verb and the object. Similar to the cognitive association performed in the human mind, the calculation determines the statistical probability100 of the object appearing with the verb. The higher the frequency of both words co-occurring in the training corpus, the more likely the object is acting to qualify the verb. The cosine similarity can, therefore, be used as a numeric interpretation of how the object is employed given the verb in the sentence.

A third puzzle came in the form of homographs. Homographs, though identical orthographically, vary in meaning (though often distinguished in pronunciation). How then could a computer distinguish between record as a noun or record as a verb? The computer scientist notes that a distinction in the meta-concepts would resolve the problem. Meta-concepts, or metadata, are the elements outside of the SVO that describe the information being conveyed. This includes in what manner and how the sentence is being expressed. How important then is meta-data to the meaning of sentences?

This was again proposed as a possible resolution when encountering deictic expressions. Deictic words – such as ‘this,’ ‘that,’ ‘here,’ or ‘there’ – rely almost exclusively on context. Consider the sentence: “At issue _here_ is not ‘carries’ at large, but ‘carries a firearm’ (emphasis added).”101 What could ‘here’ mean? To the jurist, ‘here’ represented the material facts of the case, but to the linguist, it is a limited reference to the preceding sentences. To the mathematician or computer scientist, the word _here_ represents a subjective concept for which a frame of reference and context serve to anchor it in reality.

These observations culminate into a greater question: what exactly constitutes context? Meaning hinges on the knowledge of a “word by the company it keeps.”102 If multiple interpretations of context exist, there are seemingly differing methods of arriving at ‘meaning.’

At a glance, the SVO markups are products of conversations around these patterns of dependencies within sentences. The markups represent decisions taken regarding how the sentences should be deconstructed to better articulate the interaction between subjects and objects with their verbs. In addition, the meta-data was separated from the basic SVO structures. Once the SVO markup was complete, it would form part of the training data for a decoder algorithm. The algorithm not only draws out the rules from the markup, but also other rules that the machine has gathered. This theoretically mirrors the concept of “reading between the lines.” Finally, these rules are encoded for future documents in the graph created. These efforts are premised on the idea that the markups identify only the more pertinent information in each sentence, while the algorithm detects any surrounding information.

The purpose is then to illustrate the connections and changes in the states of sentences found in the judicial decisions. In other words, it is the reconfiguration of sentences that are ostensibly void of structure, to their structurally dependent forms. In the following section, the authors articulate in detail the technical implementation of the project and demonstrate how the translation of legal text into numeric form unravels the ‘Black Box’ of instinct103 and disciplinary bounds. In the process of reducing sentences to SVO triplets, what is colloquially understood as intuition and knowledge-based expertise is revealed in a systematic form.

As discussed, numerous authors have attempted to translate natural language into numeric form using various types of algorithms. To this day, the success of these prior efforts has primarily been achieved through the use of advanced statistical modelling techniques that depend on vast amounts of data. Benefiting from these methods, the authors attempt to develop a new paradigm for natural language understanding; namely, one based on the core principles of Object-Oriented Design (OOD). The objective is to develop a preliminary model capable of ingesting a large amount of data accurately, leaving the handling of outlier cases for a later stage of analysis.

The authors build on the ideas of Walter Daelemans and Koenraad De Smedt,104 bridging concepts of OOD and linguistics. As the intention is not to be exhaustive, the table below broadly defines the analogies between OOD and linguistics that permit the translation of text.

**Object-Oriented Design**

**Concepts from Linguistics**

**Classes**

Blueprints (or prototypes) defining the characteristics and behaviors of Objects belonging to them

Hyponymy,105 items contained within a set. Defining the prototype entities which allow objects to inherit any combination of single or multiple parent characteristics.

**Objects**

Singular manifestations of a Class

Noun-phrases and lexemes corresponding to singular entities and qualities (akin to individual definitions) represented by their lemma-form.

**Methods**

A defined interaction event between Abstractions in the program. Methods must be invoked in order for them to have a role.

Clauses (narrowed down to possible permutations of (S)V(O)) – interactions between semantic entities within the text.

The syntactic _subject_ (semantic _agent_) is seen as the triggering _entity_, the (direct) _object_ is the target of the interaction, and any additional _objects_ behave as necessary inputs concerned in triggering the said interaction. The _verb_ describes what happens during the interaction.

**Variables**

Placeholders for discrete information: values, Objects

Meronomy (declaring/assigning a placeholder for a part of the whole), meronymy (defining the content in the placeholder) – assigning parts of a whole.

**Abstraction**

The definition of Classes, Objects, Methods and Variables based on the task a program will solve.

Decoupling the signifier from the signified,106 allowing for the open system nature of language and knowledge in general.

**Inheritance**

The passing on of characteristics and behaviors of a parent Abstraction onto its child

A multi-purpose mechanism allowing the modelling of linguistic phenomena, such as hyponymy, conducive to definitionism.

**Encapsulation**

The localization of characteristics and behaviors to a Class or Object

The phenomenon that allows for semantic parsing – localization of characteristics and behaviors to specific logical elements (entities) within a frame of reference.

**Polymorphism**

The ability to change any inherited characteristics and behaviors

Corresponding to the phenomena of polysemy and homographs, among others. Specifically, it allows any two entities within the same class to have different characteristics and behaviors represented by the same root word.

**Composition**

Arranging the interactions of Objects and Classes with one another; one of the aims of Composition is to reduce code redundancy

Corresponding broadly to semantics – the arrangement, hierarchy and definition of communicative rules between the logical/semantic elements (perhaps equivalent to semes or sememes) in a text. This can allow for abstraction, improving efficiency in contextual assignment.

As such, the grammatical structure of natural language is seminal to extracting its informational content. This would, in effect, permit a translation of ‘meaning’ to a form readily encodable in a programming language.

The complexity of legal concepts (i.e. the potential for multiplicity of meaning, or polysemy) requires technology capable of operating within non-singularity. Consequently, this project attempts to strike a balance between definitionism and determinism by minimizing the pitfalls of both the inefficiency and redundancy of definitionism against the brittleness of determinism. Ultimately, the goal is to secure efficient machine readability while upholding fundamental legal principles. The danger of leaning towards either the former or the latter is its adverse impact on the requirement for human intervention in the exercise of judicial reasoning. Should priority turn to definitionism, we risk creating a system that is far too complex and cumbersome to create any additional value for legal practitioners. Should priority turn to determinism, we risk creating a system that does not leave sufficient flexibility for ever-changing circumstances,107 undermining existing legal structures.

Graph databases are amenable to generating highly interconnected webs of knowledge (known as knowledge-maps), optimizing the analysis of relations between individual data points. Moreover, they account for issues of object composition, polymorphism, encapsulation, and inheritance, enabling the use of graph theory for creative analytical approaches on a larger scale. These ideas will return in the subsequent sections. Importantly, the graph works as the intermediary interface. It stores the input and analyzes the output of abstractions drawn from the developed algorithm.

In the normal reading of texts, humans typically abstract in a sequential pattern, forming a ‘world’ within our own consciousness. Each subsequent phrase that speaks to the same topic enriches the details of this ‘world,’ reinforcing it with logical constraints and other abstractions.108 This process parallels a compiler reading a piece of high-level code, such as a Python script. The input works through layers of translation before arriving at a form comprehensible to the machine. Each stage serves to ‘decompress’ the knowledge built into the language by its designers. Eventually, the language is distilled down to its most granular level: a collection of binary code.109 Phrases become a series of commands, either establishing a fact or describing an event or action.

The legal language is no different. It can be regarded as the collective effort of numerous iterations of layered abstractions rooted in social reality.110 A legal document is the written manifestation of this process, conveying abstract legal concepts in a manner that is both syntactically sound and semantically meaningful in natural language.

One of the notable drawbacks of natural language is the underlying differences in contextual knowledge of various individuals, whether it be prior experience or preconditioning. The existence of these differences manifests as “biases,” which are then inherited in physical repositories, or artifacts.111 Consequently, exposing context is often helpful in clarifying such ‘repositories of legal knowledge.’ For programmers, what is interpretable as context includes environmental details reality outside the scope of a particular program. This could mean additional software may be used by developers when putting together a system (e.g. the importing of packages in Python). The addition of these packages extends the functionality of a program beyond its defined code. For the POC, the authors use a combination of pre-defined estimators (i.e. spaCy’s neural network models for recognizing dependencies and part-of-speech tags as well as Word2Vec converters) and newly trained estimators (i.e. detecting SVO triplets) to strengthen the model with metadata relevant to statements encountered in the dataset.

Below is a pictographic interpretation of the process:

![](https://resize-v3.pubpub.org/eyJidWNrZXQiOiJhc3NldHMucHVicHViLm9yZyIsImtleSI6InFzOXFva2U4LzIxNjA1NjIzNTIyNzM4LnBuZyIsImVkaXRzIjp7InJlc2l6ZSI6eyJ3aWR0aCI6ODAwLCJmaXQiOiJpbnNpZGUiLCJ3aXRob3V0RW5sYXJnZW1lbnQiOnRydWV9fX0=)

## _a. Defining Entities (Encapsulation)_

In building reference models of reality, entities are discrete units of existence. They act as mental placeholders to facilitate explanations of interactions within the model. Encapsulation is used to localize the characteristics and behavioral characteristic of each of these entities. The entities can be grouped into categories (classes), nested and (re-)arranged in an infinite number of ways. The critical aspect of encapsulation is the architecture and its rules of performance; in other words, the process of defining entities of reference, their relations to one other, as well as their methods of interaction.

Consider the following sentence from _Bailey_ as an informative example:

> “I _use_ a gun to protect my house, but I’ve never had to _use_ it.”112  

Disregarding context, the sentence can be deconstructed into entities or methods. The entities, such as “I”, “gun”, “house,” are encoded as nouns. The methods, such as “protect” and “use,” are encoded as verbs.

Observably, the clause “I use a gun...” involves an actor (“I”) that invokes an action (“use”) on an object (“gun”).

![](https://resize-v3.pubpub.org/eyJidWNrZXQiOiJhc3NldHMucHVicHViLm9yZyIsImtleSI6IjEzZDNyc2lmLzAxNjA1NTY0MTM5MTc1LnBuZyIsImVkaXRzIjp7InJlc2l6ZSI6eyJ3aWR0aCI6ODAwLCJmaXQiOiJpbnNpZGUiLCJ3aXRob3V0RW5sYXJnZW1lbnQiOnRydWV9fX0=)

Applying the Object-Oriented approach of structuring code into classes and methods, the first phrase can be translated into the following schema:

![](https://resize-v3.pubpub.org/eyJidWNrZXQiOiJhc3NldHMucHVicHViLm9yZyIsImtleSI6InpxcnQxcnZhLzExNjA1NTY0MTM5MTc1LnBuZyIsImVkaXRzIjp7InJlc2l6ZSI6eyJ3aWR0aCI6ODAwLCJmaXQiOiJpbnNpZGUiLCJ3aXRob3V0RW5sYXJnZW1lbnQiOnRydWV9fX0=)

The components of the sentence become identifiable SVO triplets:

1.  the Subjects (invoking entity);
    
2.  the Objects (entity being acted on);
    
3.  the Verbs (method); and occasionally,
    
4.  the Prepositional Objects (additional entities describing the premise of the event/action).
    

The illustration below depicts the framework on which the algorithm is built.

![](https://resize-v3.pubpub.org/eyJidWNrZXQiOiJhc3NldHMucHVicHViLm9yZyIsImtleSI6Iml2ejdwazkxLzExNjA1NTY0MTM5MTc3LnBuZyIsImVkaXRzIjp7InJlc2l6ZSI6eyJ3aWR0aCI6ODAwLCJmaXQiOiJpbnNpZGUiLCJ3aXRob3V0RW5sYXJnZW1lbnQiOnRydWV9fX0=)

By extension of the example, subsequent phrases follow a similar breakdown, drawing connections between classes and their corresponding methods. This form of deconstruction also allows us to nest concepts and perform additional logic tests based on the connections established.

## _b. Scaling Up (Composition)_

The process of composition is akin to the first layer of translation, developing a pseudo-code script that represents a concept but expressible in a machine-readable language. The connections trace which class invoked the method “protect” on the class “house,” thereby deducing what “I” “use” to “protect” “house.” As a result, such encoding does not require vast amounts of training data. Text is immediately translated to pseudo-code, without the need for external context.

The peripheral terms present in the sentence serve to indicate higher order concepts such as enumeration, negation, time, possession and pronoun assignment: “a”, “never”, “had to”, “my”, “it”. Their presence exists to modify the fundamental building blocks of the sentence - the nouns and verbs.

## _c. Creating the Knowledge Map (Natural Language Processing)_

Whereas the task of defining individual entities and methods is relatively straightforward, creating a knowledge-map corresponding to the above schema requires the extraction of the semantic connections between them. By leveraging existing NLP tools,113 such as spaCy, in conjunction with the authors’ own SVO markups,114 the authors were able to create a corpus to train a classifier capable of detecting SVO triplets and importing them to the graph.

![](https://resize-v3.pubpub.org/eyJidWNrZXQiOiJhc3NldHMucHVicHViLm9yZyIsImtleSI6Impya3k3Ynh5LzIxNjA1NTY0MTM5MTcyLnBuZyIsImVkaXRzIjp7InJlc2l6ZSI6eyJ3aWR0aCI6ODAwLCJmaXQiOiJpbnNpZGUiLCJ3aXRob3V0RW5sYXJnZW1lbnQiOnRydWV9fX0=)

**Figure 1**: Sample Input to spaCy

The core strategy behind extracting SVO triplets lies in its linguistic deconstruction. The root of every sentence centers on the verb. Subjects (“nsubj”) and objects (predicate, “dobj”) are subordinate to verbs within the syntactic hierarchy. Therefore, through identifying the verbs of every sentence, the semantic connections are naturally found.

This method of text analysis has gained popularity with the advent of machine learning based models of NLP. These models have been trained on a sizable corpus of different expressions to perform the following tasks:

1.  Separating words from a string;
    
2.  Grouping the words into sentences;
    
3.  Assigning each word with a part-of-speech tag (Noun, Verb, Adverb, etc.); and
    
4.  Estimating each word’s syntactic parent; thereby build a syntactic tree.
    

This approach differs from other methods of semantic notation that rely solely on syntax, and less on the underlying pragmatics.115  

Between entity–method and SVO extraction, the data generated is sufficient to begin assembling together the knowledge-map. More importantly, the aforementioned process is derived entirely from the text itself. As a result, a defined cause-and-effect type algorithm can be built, executable in full or in part, tested and queried. Additional metadata such as word embeddings, sentiment analysis and recognized named entities can provide supplementary information helpful for optimizing the knowledge-map and achieving a more robust understanding of the semantic content.

## _d. Building Character; Adding Context (Inheritance and Polymorphism)_

This project considers the transformation of legal texts to an Object-Oriented-like script, effectively using ‘pseudo-code’ to depict concepts embedded within the text. In natural language, multiplicity of meaning could occur when a single concept applies to several circumstances. Different conclusions can be drawn depending on the characteristics inheritable from a parent class. To clarify, this would include determining whether a “firearm” is within the same class as “gun.” Similarly, other characteristics may include the methods or actions (verbs) invoked by a particular class. In object-oriented design, this phenomenon is known as polymorphism.

A core aspect of the translation into object-oriented form, as described in Daelemans and De Smedt’s paper, is the assumption that subclasses ‘inherit’ the characteristics of the parent class by default, unless they are hard-coded otherwise.116 In this case, characteristics and their behaviors are explicitly stated in the legal text. Consequently, if necessary and provided there are sufficient examples in the source text, as well as a threshold occurrence ratio, it will be possible to migrate certain characteristics up the inheritance hierarchy. Any such event can be signaled with a flag indicating that the presence of this characteristic is an assumption with a certain percentage occurrence rate among child objects.

![](https://resize-v3.pubpub.org/eyJidWNrZXQiOiJhc3NldHMucHVicHViLm9yZyIsImtleSI6IjV3ZmUzdW1oLzQxNjA1NTY0MTM5MTc2LnBuZyIsImVkaXRzIjp7InJlc2l6ZSI6eyJ3aWR0aCI6ODAwLCJmaXQiOiJpbnNpZGUiLCJ3aXRob3V0RW5sYXJnZW1lbnQiOnRydWV9fX0=)

**Figure 2**: Illustrating Parent and Child Classes

When SVOs have an explicit subject and object, they can be loosely connected. However, the presence of subordinate clauses in the text necessitate nesting SVOs within one another. This is represented in the pseudo-code as implicit causality. To then define the chain of causality, yet maintain the independence of each SVO, the root of a sentence must be identified. Drawing from the example, “I” must first “use a gun” in order to then “protect” “house.” This suggests that “use” is the primary connection between the SVOs as one cannot exist without the other.

![](https://resize-v3.pubpub.org/eyJidWNrZXQiOiJhc3NldHMucHVicHViLm9yZyIsImtleSI6Im5iNDQ3aGczLzQxNjA1NTY0MTM5MTc3LnBuZyIsImVkaXRzIjp7InJlc2l6ZSI6eyJ3aWR0aCI6ODAwLCJmaXQiOiJpbnNpZGUiLCJ3aXRob3V0RW5sYXJnZW1lbnQiOnRydWV9fX0=)

**Figure 3**: Illustrating causality between SVOs

Further classifications and qualifying characteristics may be important to a legal analysis. This information parallels the referencing of statutes and case law for prior interpretations of meaning. Various sources of law often create an environment for conflicting readings of a particular text. To tackle this problem, it is possible to assign an authority metric to each source, thereby establishing hierarchical structuring of the corpus. The structure behaves as a type of input when conducting an analysis, mirroring the hierarchy of legal sources.

## _a. By the Word of the Law_

With the data loaded into the graph, so began this stage of analysis. The primary method of interacting with the knowledge graph is through the query function. Each query attempts to build one or more paths between two entities, with specific constraints along its path. This is the programming equivalent of writing tests for a piece of code. The knowledge graph receives a query and returns a response that follows the reasoning of human observers. Once the knowledge graph has acquired sufficient data, the intention is to develop a user interface capable of answering ‘legal’ questions posed by its users.

An invaluable tool used in this task is the Cypher query language. This language permits the formulation of queries based on the paths present within the data. The choice of constraints for each query will initially be hard-coded. Nevertheless, it may be possible to then transfer the process to machine learning should sufficient data be gathered.

The goal behind this approach is to improve upon the standard statistically driven paradigm to allow the inference of logical conclusions from the text.

Consider a user query: “Describe the interactions involving a firearm.”

![](https://resize-v3.pubpub.org/eyJidWNrZXQiOiJhc3NldHMucHVicHViLm9yZyIsImtleSI6IjhpdGUxcG51LzQxNjA1NTY0MTM5MTc2LnBuZyIsImVkaXRzIjp7InJlc2l6ZSI6eyJ3aWR0aCI6ODAwLCJmaXQiOiJpbnNpZGUiLCJ3aXRob3V0RW5sYXJnZW1lbnQiOnRydWV9fX0=)

**Figure 4**: Sample output from Neo4j Graph

With a user interface, the authors envisage that any question can be deconstructed in the same way as the training dataset. In this case, the algorithm should return the associations of entities and methods affiliated with “use” and “firearm.” The interface will attempt to: (1) link the entities in the question, using the data in the graph; (2) gather any conditions and constraints encountered during processing; and (3) return the relevant information as a series of possible paths taken within the graph, resulting in a list of phrases sorted by relevance (e.g. “use is active employment”).117 In effect, legal judgments are reconfigured into machine readable form to identify the meaning of the text. The graph acts as a signpost for legal actors in forming definitions found in judicial decisions; thereby augmenting legal reasoning by leveraging the efficiency and power of computational analysis.

## _b. By the Sixth Sense_

On the other hand, there has been a latent understanding that intuition plays a role in the rendering of judicial decisions.118 The techniques used in the authors’ approach, in fact, account for instinct. The parsing of legal texts requires two types of algorithmic methods: (1) analytical; (2) and numerical.

The former serves to build a rigid structure from text and establish a hierarchy of semantic content on the basis of clearly defined criteria. This was demonstrable in the use of the graph database. The latter leverages the statistical modelling principles of neural networks. Similar to impulses attributable to intuition, the weight of each neuron in a neural network can be viewed as an abstract meta-concept, too complex to express tangibly. A parallel can be drawn between the phenomenon of a “gut feeling” and the internals of a neural network, as trends embedded within a dataset are sorted into an array of codependent activation values. This means that any data present on the graph can be fed into customized machine learning algorithms to approximate human ‘intuition.’ Together, the authors could emulate several forms of legal reasoning that often underlie judicial decisions.

## _c. Between Implementation and Effect_

The impact of translation has inadvertently exposed the logic of legal reasoning. Whether it is judicial intuition or syllogistic application, Holmes’s paradox remains relevant. Words of legal text do, in fact, intrinsically embody meaning. The sphere of legal knowledge exists well within the sentences of judicial decisions. This is owed to the interpretation and conceptualization of precedent. The POC has observed that the use of precedent is not a procedural legal tool but a substantive one. Its application is to uphold the appearance of methodological consistency within the body of law. Yet, fundamentally, its use is to substantiate the authority of legal texts.

More importantly, precedent recognizably functions in an asymmetrical, as opposed to syllogistic, manner.119 To recall, _Bailey_ does not apply the plain meaning of ‘active employment,’ but instead constructs an alternative legal meaning to equate “active” to “operative factor.”120 In other words, in accordance with _Smith_ and _Bailey_, the use of a firearm includes bartering; and as such, the trading of a firearm is an ‘operative’ component to a drug-trafficking crime. These definitions are not logically deduced. Instead, they seek to reinforce a specific legal framing. Arguably, then, the use of precedent is not to follow past decisions, but to determine how to align a new decision with them. This was integral to incorporate into the graph, as the semantic content drew from legal taxonomy.

The result of translating legal text in the manner described in Section 4 corroborates that legal language is self-referential and consistent. The law pushes outward by looking inward. In deconstructing legal judgments to its constituent components, the process of applying precedent evidently evolves: from syllogistic application to a framework of extraction.

## _d. On New Terrain_

One of the aims of this research is to test the feasibility of expanding this approach — to translate natural language into code over a bigger corpus. In navigating the knowledge graph, the authors intend to apply elements of deontic logic121 as an initial approach to the causal resolution of sequential events. Each SVO will be attributed variables corresponding to whether or not it is possible and/or necessary. This determines whether it can or should be invoked when resolving a query. Furthermore, the data can be used as input for graph-based algorithms when carrying out large-scale data analysis.

The next phase considers how far this model can extract the semantic content of judicial decisions. The application of the knowledge-map will be useful in this regard. Having laid the groundwork, this project will progress toward detecting logically conflicting statements, adding dimensions such as time and conditionality, as well as testing more nuanced relationships found in the text.

First, the technical implementation of the POC alludes to the possibility of conflicting interpretations of legal text fostered by the array of legal sources. Alternatively, the presence of dissenting opinions could pose a similar issue (i.e. opposing views on a subject matter). This may appear on the graph as identical SVOs — one in the affirmative and the other in the negative. As all SVOs will be assigned an authority metric based on the source of law, it may be possible to include an additional marker indicating: (1) the opinion of the court; and (2) dissenting or concurring opinions. This would enable navigating any query along the SVO by the highest level of authority, while equally maintaining any ‘lower level’ paths that would offer informative insight.

Next, some SVOs denote conditions that invoke a parent SVO (e.g. phrases beginning with “if,” “when,” “whether,” etc.). Such phrases are equivalent to tests that must be satisfied by a different set of nodes within the graph, providing answers to specific queries. By labelling these SVOs as conditional within the graph, it is possible to connect them as preconditions for the invocation of another SVO. For example, “if one ‘carries a firearm,’ then he or she knowingly possesses it.”122 The “if” statement denotes a logical test to determine whether the firearm is “knowingly possessed.”

Owing to their subjective and relative nature, it remains undecided whether and how to group seemingly similar modifiers. Modifiers (i.e. adjectives and adverbs) used to qualify the root of each component in an SVO are currently treated as separate objects for the purpose of comparison. For the purposes of this project, modifiers can enable a connection between semantics and pragmatics embedded within the legal text. As this project uses legal text to generate an abstract frame of reference, the decision to treat modifiers as purely subjective results in a subjectively impartial point of view.

Consider the phrase “a fast dog might run alongside a slow car.” The modifiers ‘fast’ and ‘slow’ can apply to the same quantity of an entity (e.g. degree of speed). It is only sensible then to compare/connect isolated relative terms directly within the bounds of their entity’s relative frame of reference with regards to the same modifier. Having narrowed the scope, it becomes reasonable to connect purely subjective qualities (e.g. dog: “fast”→ is\_not/faster\_than → “slow”).123 As well, it is necessary to distinguish the subjective and objective qualities of any entity with the subjective dependent on the objective. The establishment of this connection can serve to anchor subjective qualities from multiple sources within a common empirical frame of reference, rendering them comparable. Should the legal text provide sufficient information, the authors intend to create a mechanism to affix any subjective quality to a quantifiable framework (e.g time,124 position, etc.).

Lastly, it is possible to build predictive modules using graph theory that will infer correlations, trends, and common behaviors between the nodes and edges within the graph, as well as integrate external factors (e.g. socio-economic factors, business metrics, etc.). For instance, others may have interest in exploring the importance of certain SVOs in the graph relevant to their structural surroundings. The goal could be to analyze the patterns in the paths connecting SVOs. This is one area where a structural analysis, using graph theory, is helpful. Random walks125, popular in machine learning, could analyze the latent representation of nodes within a graph. In this case, randomized paths initiated at a subset of nodes would be generated to collect sets of metadata used to infer trends within the structure of the graph. This has the potential to describe how the relationships between related SVOs evolve in legal discussions and could also illuminate underlying real-world effects.

Applications of such an approach could include:

-   Evaluating the key logical paths characteristic of a precedent;
    
-   Highlighting areas of increased ambiguity within legal reasoning;
    
-   Estimating and counter-acting implicit bias; and
    
-   Identifying phenomena such as feedback loops and contradictions.
    

The fundamental question asked by this project concerns whether meaning draws association from the language in which it is seated; that in changing the language, meaning will naturally be reconceptualized. The test to translate natural language into numeric form is not novel. In fact, it follows an ancestry of applying mathematical precision to legal expression. This paper has sought to experiment with the conversion of legal texts into algorithmic form. More importantly, the authors attempted to capture legal concepts and the processes involved in legal reasoning. The deconstruction of natural language phrases to SVOs atomized sentences into their bare structures, forcibly exposing connections integral to the formation of concepts. As the authors aimed to reconcile syntax with semantics, structure became indistinguishable from content.

Inadvertently, the POC has demonstrated that, though form is seminal to the adjudicative exercise, the logic embedded within legal texts does not necessarily behave syllogistically. Instead, legal concepts appear to evolve sporadically. This sporadicity, however, is not synonymous to randomness. Rather, the development of the law draws from introspection and uses precedent to substantiate its authority. Teasing at Holmes’s paradox, the law approaches consistency not in form, but in substance. As opposed to syllogistic application, meaning is found through a process of extraction.

The next phase of the project will bring forth a deeper breakdown of legal texts, focusing on higher levels of abstraction (i.e. trends latent in meta-concepts), and more complex grammatical resolutions found in natural language. From a broader perspective, the project has inspired the authors to advance toward a ‘White Box’ solution. The aim is to strengthen the understanding of legal texts, providing better roadmaps and guiding users toward more consistent interpretations of judicial decisions. It is an evolution of legal reasoning that heightens transparency and auditability by unpacking juridical truths and structuring intangible legal narratives. This would result in improved quality of legal analysis and increased accessibility to society.

As opposed to “grafting new technology onto old working practices,”126 our approach enables a new embodiment of precedent. We can harness the future through preserving the past. The integration of computational technology in law disrupts conventional legal mechanics, while maintaining the function of law. The authors anticipate a Bilbao effect: that the thoughtful marriage of old and new architectures sparks transformation.

_Megan is a PhD Candidate in Law and Lecturer at Sciences Po_

_Dmitriy is a Senior Legal Data Scientist at Simmons Wavelength Ltd._

_Avalon is a MASt in Mathematics at the University of Cambridge_

_Adam is a BA in Linguistics at the University of Cambridge_
