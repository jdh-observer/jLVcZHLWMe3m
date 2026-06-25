---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.19.3
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

<!-- #region editable=true slideshow={"slide_type": ""} tags=["title"] -->
# Semantic Textual Similarity for Oral Tradition Analysis: An AI-Augmented Isnād-cum-Matn Analysis of Early Islamic Hadith
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} tags=["contributor"] -->
### Seyfeddin Kara [![orcid](https://orcid.org/sites/default/files/images/orcid_16x16.png)](https://orcid.org/0000-0002-0651-0859) 
University of Groningen
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} tags=["contributor"] -->
### Ayub Nur [![orcid](https://orcid.org/sites/default/files/images/orcid_16x16.png)](https://orcid.org/0009-0000-2533-2317) 
Tufts University
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} tags=["copyright"] -->
[![cc-by](https://licensebuttons.net/l/by/4.0/88x31.png)](https://creativecommons.org/licenses/by/4.0/)
©<Kara and Nur>. Published by De Gruyter in cooperation with the University of Luxembourg Centre for Contemporary and Digital History. This is an Open Access article distributed under the terms of the [Creative Commons Attribution License CC-BY](https://creativecommons.org/licenses/by/4.0/)

<!-- #endregion -->

```python editable=true slideshow={"slide_type": ""} tags=["cover"]
from IPython.display import Image 
metadata={
    "jdh": {
        "module": "object",
        "object": {
            "type":"image",
            "source": [
                "cover"
            ]
        }
    }
}
display(Image("media/cover.jpg"), metadata=metadata)
```

<!-- #region editable=true slideshow={"slide_type": ""} tags=["keywords"] -->
**Keywords**: Islamic origins, Hadith, Arabian context, Medina, sacred spaces, isnād-cum-matn analysis, CollateX, semantic textual similarity, LLM
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} tags=["abstract"] -->
**Abstract**:
This article introduces a novel, AI-based methodological workflow designed for semantic textual criticism in traditions characterised by substantial oral variation. Focusing on the contested question of the genesis of Medina's sacred status, the study first employs isnād-cum-matn (a historical-critical method that integrates transmission analysis with form and redaction criticism) to reconstruct the earliest strata of Hadith narratives. This manual process revealed that references to Medina’s sanctity are plausibly traceable to the era of the Prophet Muhammad. Crucially, the core contribution lies in augmenting this traditional philology with a quantitative, AI-enhanced model. This model, deployed for the first time in Islamic studies, quantifies Semantic Textual Similarity (STS) among Hadith variants. It provides a complementary quantitative method that overcomes a fundamental limitation of traditional digital collation (such as CollateX), which struggles to align texts that diverge lexically while preserving identical meaning.

The resulting strong convergence between the human-derived clusters and the machine-generated semantic scores validates the efficacy of the traditional method and establishes a new paradigm for computational approaches to textual studies in the Digital Humanities. This lightweight, replicable framework allows researchers to conduct meaning-based collation on complex, multilingual textual corpora, moving beyond character-level matching to recover the conceptual integrity of orally transmitted sources.
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} -->
## Introduction
<!-- #endregion -->

<!-- #region citation-manager={"citations": {"ese3x": [{"id": "167992/4XF2D7I4", "source": "zotero"}], "liqif": [{"id": "167992/68L2J228", "source": "zotero"}], "uhumx": [{"id": "167992/3FBRBUHQ", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} -->
Analysing textual variants is a fundamental task for scholars working with historical texts, yet it remains tedious and often fails when dealing with traditions of oral origin, namely Hadith. This article proposes a solution to a key challenge in digital textual criticism: the inability of character-level collation tools to recognise semantic equivalence in texts that have undergone substantial paraphrasing. We achieve this by integrating Large Language Models (LLMs) and Semantic Textual Similarity (STS) analysis into a traditional philological workflow, using a complex corpus of early Islamic Hadith literature as a critical test case. The ongoing debate over the sanctification of Medina provides the ideal case study for this new method, as it hinges on the reliability of highly variant, orally transmitted narratives. 

The sacred status of Medina, recognised in Islamic tradition as a ḥaram (sanctuary), constitutes a central issue of scholarly inquiry, primarily concerning its genesis and early institutionalisation. A seminal contribution to this field is Harry Munt's The Holy City of Medina, which argues that the city's sacred identity crystallised gradually over centuries through shifting political and religious agendas, rather than originating with the Prophet Muhammad himself. Munt grounds this position in the relative absence of explicit references to Medina's sanctity in near-contemporary sources, and reinforces it by drawing on subsequent legal debates — particularly among Ḥanafī jurists — to show that the city's privileged status was never universally agreed upon. He does acknowledge that the so-called "Constitution of Medina" refers to a ḥaram, but treats this as a politically motivated gesture enabling the Prophet to assert independence from Quraysh, rather than evidence of genuine religious sanctification. <cite id="ese3x"><a href="#zotero%7C167992%2F4XF2D7I4">(Munt, 2014)</a></cite>

While Munt's work offers an important reinterpretation of Islamic sacred geography, its methodology reveals a critical problem: he dismisses the broader Hadith-based accounts as retrospective theologising while simultaneously treating the "Constitution of Medina" as historically transparent evidence which is a selective standard that requires justification. More broadly, his account sits uneasily with what is known of the Arabian context of early Islam, in which the sanctification of space was neither novel nor exclusively political. The concept of sacred precincts serving as protected spaces was already a well-established tradition on the eve of Islam. <cite id="liqif"><a href="#zotero%7C167992%2F68L2J228">(Webb, 2024)</a></cite> Even Musaylama (d. 12/633), who claimed to rival Muhammad's prophethood and attracted strongly negative portrayals in Muslim sources, "erected a safe area (ḥaram) surrounding certain places inhabited by his allies." <cite id="uhumx"><a href="#zotero%7C167992%2F3FBRBUHQ">(Kister, 2018)</a></cite> It would be difficult to argue that Muslims projected their own religious practices onto a figure regarded as a dangerous heretic. This context, examined in greater detail in Section 1 below, forms the historical backdrop against which the Hadith corpus under study must be read.

The article is structured in two parts. Section 1 opens with a brief account of the scholarly context before implementing the isnād-cum-matn analysis manually, establishing the core historical findings. Section 2 then applies an AI-based semantic similarity model to the same textual corpus for quantitative verification and comparative evaluation. This integration provides a complete framework that validates the efficacy of traditional scholarly methods while demonstrating the potential of LLM-assisted analysis to enhance them.
<!-- #endregion -->

<!-- #region citation-manager={"citations": {"9pcbf": [{"id": "167992/DLCIQE2K", "source": "zotero"}], "p2d7b": [{"id": "167992/2QDX8ENH", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
## MANUAL STUDY OF THE HADITHS THROUGH ISNĀD-CUM-MATN ANALYSIS ##
### Scholarly Context
The ḥaram status of Mecca provides the precedent against which Medina's sanctification must be understood. When the relevant Quranic verses are analysed — namely Q. 2:125–129 — it is evident that the entire practice of the pilgrimage ritual is grounded in the Abrahamic experience, formalising Abraham's dedication and replicating his perceived state of perfection. This wider Abrahamic heritage also reflects a well-known pattern in the ancient Near East, wherein locations where a deity manifested were sanctified by that presence, and the altars Abraham built in the locations mentioned in biblical sources were accordingly regarded as sacred. <cite id="9pcbf"><a href="#zotero%7C167992%2FDLCIQE2K">(Pagolu, 1998)</a></cite> Far from being an innovation, Muhammad's reported sanctification of Medina would have been legible within this inherited context, not a significant departure from what his contemporaries already believed about Abraham and the consecration of Mecca.

It is within this context that a distinct group of Hadiths reports that Muhammad explicitly consecrated Medina in emulation of Abraham's consecration of Mecca. A representative example is recorded in Mālik b. Anas's Muwaṭṭaʾ:

> Narrated to me by Yaḥyā on the authority of Mālik, on the authority of ʿAmr mawlā (client) of Muṭṭalib, on the authority of Anas b. Mālik: "The Messenger of God saw Mount Uḥud, and he said: 'This is a mountain that loves us, and we love it. O God, Abraham sanctified Mecca, and I sanctify what is between its (Medina) two lava fields.'"<cite id="p2d7b"><a href="#zotero%7C167992%2F2QDX8ENH">(Ibn Anas, 1994)</a></cite> (Hadith no: 1645)

The presence of this Hadith and its variants suggests that, analogous to Abraham's sanctification of Mecca as stated in Q. 2:124–128, the Prophet conferred a corresponding status upon Medina. To assess this claim historically, the narratives must be traced back through their chains of transmission, and if an original Hadith underlies these variants, it must be reconstructed. That is the task of the philological analysis that follows.

### The Hadith Variants and Method:
The Hadith and its variants, under study, all include the element of comparison between Abraham’s sanctification of Mecca and Muhammad’s sanctification of Medina. The study involves initially clustering the relevant Hadith and examining the chains of transmission (isnāds), followed by an analysis of the texts (matns) to establish the history of the transmission process and, if possible, corroborate textual affinity. There are fifteen variants of this Hadith, eleven of which were recorded in Sunni sources such as Muslim’s (d. 261/875) Ṣaḥīḥ, ʿAbd al-Razzāq’s (d. 211/826) Muṣannaf, Ibn Abī Shayba’s (d. 235/849) Muṣannaf, Mālik’s (d. 179/795) Muwaṭṭaʾ, Tirmidhī’s (d. 279/892) Sunan, and Ibn Mājah’s (d. 209/824) Sunan. Furthermore, five of them are in Shiʿi sources, which are al-Kulaynī’s (d. 329/941) al-Kāfī, al-Ṭūsī’s (d. 460/1067) Tahdhīb al-Aḥkām and al-Amālī. Below are the chain-of-transmission diagram and the corresponding textual variants, titled in Tables 1 and 2.
<!-- #endregion -->

```python editable=true slideshow={"slide_type": ""} tags=["figure-isnadtree-*"]
from IPython.display import Image 
metadata={
    "jdh": {
        "module": "object",
        "object": {
            "type":"image",
            "source": [
                "Chain of Transmission Diagram"
            ]
        }
    }
}
display(Image("media/Picture 1.png"), metadata=metadata)
```

```python editable=true slideshow={"slide_type": ""} tags=["table-textualAnalysis-*"]
from IPython.display import Image 
metadata={
    "jdh": {
        "module": "object",
        "object": {
            "type":"image",
            "source": [
                "The clustered textual variants of the Hadith"
            ]
        }
    }
}
display(Image("media/Picture 2.jpg"), metadata=metadata)
```

<!-- #region citation-manager={"citations": {"fu16o": [{"id": "167992/MJWFGGMR", "source": "zotero"}], "i9sla": [{"id": "167992/MJWFGGMR", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} -->
According to a preliminary study of Table 1, the variants appear to have been transmitted through six possible transmission lines from the Prophet. The strands are transmitted through ʿAlī b. Abī Ṭālib, Abū Hurayra, Abū Saʿīd, Jābir, Anas b. Mālik and ʿAbdullāh b. Zayd b. ʿĀṣim. The relevant texts of the variants can be found in the above-mentioned Table 1, which we have also divided according to textual elements and indicated the sources in which they were recorded. 

Out of these six main transmitters, namely ʿAlī b. Abī Ṭālib and Abū Hurayra seem to have disseminated the report to multiple individuals. In one of these reports, the transmission occurs via ʿAmr. Thus, from the initial observation of the chains of transmission, we identified three apparent Common Links (henceforth, CLs) which refer to key transmitters that disseminate the Hadith to multiple individuals for the first time. These are ʿAlī, Abū Hurayra, and ʿAmr. These preliminary observations are subject to confirmation or rejection by the study’s conclusion, based on the comparative analysis of the chains and the texts. In two of the isnāds, the sixth Shiʿi Imām Jaʿfar al-Ṣādiq’s source was listed as ʿAlī. However, in the remaining two, he directly reports from the Prophet. It is plausible that this constitutes a family chain, with Jaʿfar al-Ṣādiq receiving this report through the Prophet’s family. Nevertheless, this assumption requires further study and textual evidence, which we intend to pursue. To improve the clarity of the research, we will categorise the variants into three clusters based on the apparent CLs. Additionally, we will group the remaining variants that do not have a CL as Miscellaneous. 

Finally, in terms of the chain of transmission, one must state that G. H. A. Juynboll, who is one of the most sceptical hadith scholars, expected an authentic hadith to spread out of the Prophet immediately to multiple transmitters, suggesting at least three CLs.<cite id="fu16o"><a href="#zotero%7C167992%2FMJWFGGMR">(G.H.A. Juynboll, 1993)</a></cite>  Such a transmission pattern should exhibit at least three Partial Common Links  (PCLs), further wide disseminators of the Hadith after CLs, which are vital for verifying the historical accuracy of a cluster, while their absence implies the creation of fabricated traditions.<cite id="i9sla"><a href="#zotero%7C167992%2FMJWFGGMR">(G.H.A. Juynboll, 1993)</a></cite>  As it will be seen below, if Juynboll’s criteria are applied to the chain of transmission of the Hadith at hand, it may primarily satisfy Juynboll’s expectations. It spreads out right after the Prophet with six transmission lines, after which it was further disseminated by three apparent CLs and then another three PCLs. However, for isnād-cum-matn analysis, these criteria are not enough to date and/or reconstruct a hadith, as they are only related to the assessment of the chains of transmission. There must also be textual affinity correlating with the transmission history, which will be studied in the following. 
<!-- #endregion -->

<!-- #region citation-manager={"citations": {"0eba8": [{"id": "167992/6WD4V9CN", "source": "zotero"}], "0ur7f": [{"id": "167992/8HXPRMZX", "source": "zotero"}], "2h7oh": [{"id": "167992/SJB8HG5R", "source": "zotero"}], "6nign": [{"id": "167992/Y58CKY2X", "source": "zotero"}], "aelpp": [{"id": "167992/S4PK974Q", "source": "zotero"}], "cfocg": [{"id": "167992/6WD4V9CN", "source": "zotero"}], "drpcn": [{"id": "167992/P3MZ8VM9", "source": "zotero"}], "f9wqh": [{"id": "167992/6NDAKMYI", "source": "zotero"}], "gn2d8": [{"id": "167992/NSGSXDPG", "source": "zotero"}], "l8xw6": [{"id": "167992/8UA474YW", "source": "zotero"}], "lrlfq": [{"id": "167992/RFDAPQNS", "source": "zotero"}], "nhctk": [{"id": "167992/6NDAKMYI", "source": "zotero"}], "rsul9": [{"id": "167992/WDVWH94N", "source": "zotero"}], "s0y9m": [{"id": "167992/RFDAPQNS", "source": "zotero"}], "yl3ng": [{"id": "167992/K6XYNART", "source": "zotero"}], "z2wlu": [{"id": "167992/8UA474YW", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} -->
### The ʿAlī b. Abī Ṭālib Cluster
#### Isnād analysis:
There are six variants potentially transmitted through ʿAlī, one in Tirmidhī’s Sunan and the remaining five in Shiʿi sources, such as al-Kulaynī’s al-Kāfī, al-Ṭūsī’s Tahdhīb al-aḥkām, and al-Amālī. However, as noted earlier, two of the Shiʿi variants do not mention ʿAlī’s name, while others identify him as the transmitter of the episode from the Prophet. While there may be potential explanations for this, it is important to note that there is a Sunni chain that can be independently verified, providing additional support for the assessment of the variants. However, at the same time, the presence of the Sunni chain of transmission attributed to ʿAlī introduces the possibility that the Shiʿi chains might have been forged. It is probable that these chains were sourced from Sunni books, the non-Shiʿi transmitters omitted, and then attributed to al-Jaʿfar from whom to ʿAlī and/or the Prophet. Furthermore, the inclusion of Sayf b. ʿAmīra in one of the transmission lines makes this possibility palpable, given his family’s reputation for plagiarising Sunni traditions and incorporating them into Shiʿi collections by way of forging the Shiʿi chain of transmissions.<cite id="l8xw6"><a href="#zotero%7C167992%2F8UA474YW">(Kuzudişli, 2015)</a></cite>  Be that as it may, aside from Sayf b. ʿAmīra, the tradition is transmitted through two other Shiʿi transmitters. Therefore, there seems to be sufficient material to assess whether the Shiʿi transmitters adopted a potentially plagiarised version of this report. Due to the length of the study, we will be as brief as possible with the isnād analysis and only examine the significant issues.

The Variant 1 (V1) in this cluster is recorded in Tirmidhī’s Sunan or Jāmiʿ.<cite id="yl3ng"><a href="#zotero%7C167992%2FK6XYNART">(al-Tirmidhī, No date)</a></cite>  This chain reaches ʿAlī and ʿAlī from the Prophet without problem. Variant 2  (V2)<cite id="cfocg"><a href="#zotero%7C167992%2F6WD4V9CN">(al-Kulaynī, 1986)</a></cite> was recorded in Muḥammad b. Yaʿqūb al-Kulaynī’s (d. 329/941, Qom) al-Kāfī <cite id="0ur7f"><a href="#zotero%7C167992%2F8HXPRMZX">(Andrew J. Newman, 2000)</a></cite> and can be traced back to six Imām Jaʿfar al-Ṣādiq, whose teknonymy is Abū ʿAbdillāh. He resided mainly in Medina and travelled to Kufa <cite id="s0y9m"><a href="#zotero%7C167992%2FRFDAPQNS">(Modarressi, 2022)</a></cite>.  His father Muḥammad al-Bāqir (d. 114/733, Medina) was the fifth Imām. Al-Bāqir met some of the Companions of the Prophet, such as Jābir b. ʿAbdillāh and ʿAbdullāh b. ʿUmar.<cite id="lrlfq"><a href="#zotero%7C167992%2FRFDAPQNS">(Modarressi, 2022)</a></cite>  

Variant 3<cite id="2h7oh"><a href="#zotero%7C167992%2FSJB8HG5R">(al-Ṭūsī, 1986)</a></cite>  (V3) is found in al-Shaykh al-Ṭūsī’s <cite id="drpcn"><a href="#zotero%7C167992%2FP3MZ8VM9">(Ansari &#38; Schmidtke, 2020)</a></cite> (d. 460/1067, Baghdad and Najaf) Tahdhīb al-aḥkām. He likely copied it from al-Kāfī as the chain is identical to the previous chain. Al-Ṭūsī recorded Variant 4 (V4) in his al-Amālī <cite id="rsul9"><a href="#zotero%7C167992%2FWDVWH94N">(al-Ṭūsī, 1995)</a></cite> (Hadith no: 1416).  This chain reaches Jaʿfar al-Ṣādiq through a different chain of transmission. Aḥmad b. Rizq heard it from ʿĀṣim b. ʿAbdulwāḥid al-Madāʾinī (d. early second/eighth century, Baghdad), who is an unknown person. He rarely appears in sources like Muḥaddith Nurī’s (d. 1320/1902) Mustadrak al-wasāʾil and reports from Jaʿfar al-Ṣādiq <cite id="6nign"><a href="#zotero%7C167992%2FY58CKY2X">(Ḥusayn b. Muḥammad Taqī Nurī al-Ṭabarsī, 1987)</a></cite>. He claims to have heard this account from Jaʿfar. Although he might have obtained it from Jaʿfar, the fact that he was an unidentified individual makes it problematic. Nonetheless, considering that Aḥmad b. Rizq was also a companion of Jaʿfar, which mitigates the issue as Aḥmad b. Rizq could have directly heard the report from Jaʿfar.

Variant 5 (V5) is found in al-Kulaynī’s al-Kāfī <cite id="0eba8"><a href="#zotero%7C167992%2F6WD4V9CN">(al-Kulaynī, 1986)</a></cite> (Bāb Taḥrīm al-Madīna). Kulaynī’s source for this variant is his most cited sources, referred to as “several of our associates” (ʿiddatun min aṣḥābinā). This expression is often employed in al-Kāfī, and he did not mention to whom it refers <cite id="f9wqh"><a href="#zotero%7C167992%2F6NDAKMYI">(Māzandarānī, 2009)</a></cite>.  Therefore, it is better to be cautious here as al-Kulaynī’s source is not specific. It is possible to deduce information about his informant by examining the subsequent transmitters in the chain. The source of the unidentified informant is Aḥmad b. Muḥammad (d. the end of the second/eighth century, Kufa and Qom)<cite id="gn2d8"><a href="#zotero%7C167992%2FNSGSXDPG">(Abū al-Qāsim al-Khū’ī, No date)</a></cite>.  He was considered a companion of Imām al-Kāẓim and Imām al-Hādī (d. 254/868, Medina and Samarra). 

Whenever “several of our associates” come between al-Kulaynī and Aḥmad b. Muḥammad, it is argued, refers to one of these informants of al-Kulaynī: ʿAlī b. Ibrāhīm b. Hāshim, ʿAlī b. Muḥammad b. ʿAbdillāh b. Adhīna and Aḥmad b. ʿAbdillāh b. Umayya and ʿAlī b. Ḥasan.<cite id="nhctk"><a href="#zotero%7C167992%2F6NDAKMYI">(Māzandarānī, 2009)</a></cite>  These names are inferred, and it remains uncertain whether he indeed received this specific variant from any of these individuals. Nonetheless, it is a possibility. In any case, it is improbable that al-Kulaynī fabricated this information, because he transmitted the same variant through a different chain of transmission. 

Aḥmad b. Muḥammad heard it from ʿAlī b. al-Ḥakam (d. c. 220/835, Kufa).  He heard the variant from Sayf b. ʿAmīra (d. c. mid-second century, Kufa and Basra). Sayf b. ʿAmīra and his family were associated with hadith forgery <cite id="z2wlu"><a href="#zotero%7C167992%2F8UA474YW">(Kuzudişli, 2015)</a></cite>.  Although Sayf b. ʿAmīra was a companion of Jaʿfar al-Ṣādiq;  he did not hear it directly from him, but through Ḥasan b. Mihrān of Kufa.  Therefore, despite some problems in this chain, in theory, it is possible that this chain was sound. However, the problems noted in the chain require scrutiny in the textual analysis. 

Variant 6 (V6) was again recorded in al-Shaykh al-Ṭūsī’s Tahdhīb al-aḥkām <cite id="aelpp"><a href="#zotero%7C167992%2FS4PK974Q">(Tafrīshī, 1990)</a></cite>.  He copied the variant from al-Kulaynī, as the rest of the chain is identical to the previous one. Based on the isnād analysis alone, some variants may reach back to the time of ʿAlī (d. 40/661), while the majority can be dated to the death of Jaʿfar in 148/765. 

<!-- #endregion -->

<!-- #region citation-manager={"citations": {"141n5": [{"id": "167992/VETMPFAK", "source": "zotero"}], "c6mjn": [{"id": "167992/VETIM69M", "source": "zotero"}], "fifbc": [{"id": "167992/VJ6FEHFL", "source": "zotero"}], "ni9gh": [{"id": "167992/SMDM2A7L", "source": "zotero"}], "nn489": [{"id": "167992/RM5T3QX9", "source": "zotero"}], "qa3wl": [{"id": "167992/6WD4V9CN", "source": "zotero"}], "v37w1": [{"id": "167992/XVZVIRFZ", "source": "zotero"}], "xgjay": [{"id": "167992/K6XYNART", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} -->
#### Textual analysis 
According to V1 ʿAlī and a group of Muslims, accompanied by the Prophet, arrived in the area called al-Suqyā whihc is an area situated in Medina, housing both a well and a mosque. Saʿd b. Abī Waqqāṣ, who was a prominent companion of the Prophet, was also present in the group. The well and its surroundings were owned by Saʿd b. Abī Waqqāṣ,  so it is possible that he was accompanying the Prophet <cite id="fifbc"><a href="#zotero%7C167992%2FVJ6FEHFL">(al-Baṣrī, 1979)</a></cite>. ʿAlī narrated that the Prophet requested water for ablution: “We went out with the Messenger of God, until we reached al-Suqyā of al-Ḥarra. There was Saʿd b. Abī Waqqāṣ and the Messenger of God said: ‘Bring me ablution (wuḍuʾ) [water]; he performed ablution.’” Upon performing ablution, he stood facing the qibla (direction of prayer) and uttered: 

> O God! Abraham was your servant and friend; he invoked blessings for the people of Mecca. I am your servant and your messenger, and I invoke blessings for the people of Medina. Bless their measures and their weights (mudd and sāʿ) as you blessed the people of Mecca and bestow upon them two-fold blessings <cite id="xgjay"><a href="#zotero%7C167992%2FK6XYNART">(al-Tirmidhī, No date)</a></cite> (Hadith no: 3914).

V1 documented in Tirmidhī’s Sunan includes four distinct textual elements. These include:

•	The Prophet’s performance of ablution in al-Suqyā.
•	The Prophet’s turn to the qibla and mentioning of Abraham and his blessing (baraka) of the people of Mecca. (دعا لأهل مكة بالبركة)
•	A comparison of Abraham’s blessing of Mecca with the Prophet’s blessing (tabāraka) of Medina. 
 (أدعوك لأهل المدينة أن تبارك)
•	The mention of saʿ and mudd (saʿ and mudd were units of measurement commonly used in trade, commerce, and daily life during that era) measurements. 

V1’s structure indicates a comparison between Abraham’s blessing and/or sanctification of Mecca and Muhammad’s effort to emulate this precedent. It also explicitly references Quranic verses Q. 3:96 (“It [Mecca] is a blessed (mubārakan) place”) as these are the derivatives of the root word b-r-k. This establishes an apparent concurrence between the Quranic verses and the Prophet’s Hadith, which leads to two possibilities. First, the Hadith is a later construction to synchronise the Hadith of the Prophet with the Qur’an. If it were the case that later Muslims forged this report, and in accordance with Munt’s theory, the objective was to bolster the sanctification of Medina by lending it greater legitimacy through the Qur’an. The second possibility is that the narrative authentically reflects the teachings of the Prophet. He genuinely sought to emulate what he thought was the Abrahamic tradition, thus sanctifying Medina under that aspiration and tradition. 

In this vein, aside from the derivatives of the root word b-r-k, the use of the phrase “slave” (ʿabd) and “friend” (khalīl) to refer to Abraham is significant as this terminology is used in the Qur’an to refer to Abraham (Q. 6:88; Q. 38:45). The term friend (khalīl) is also used to refer to Abraham in the Qur’an (Q. 4:125). While “slave” (ʿabd) was used for the other prophets, Zacharia (Q. 19:2), Jesus (Q. 19:30; Q. 43:59), David (Q. 38:17), Solomon (Q. 38:30), Noah (Q. 54:9), friend (khalīl) is only used for Abraham. Therefore, it is a specific Quranic reference to Prophet Abraham.

V1 also emphasises that the sanctification of Mecca was a response to Abraham’s request. Despite the use of the word “blessing” (al-baraka) instead of a more technical term like “sanctuary” (ḥaram), the progression of the narrative, beginning with the Prophet performing ablution, then standing facing the qibla and uttering these words, refers to a ritual sanctification of the city of Medina. In the second section, the comparative narrative, Muhammad identifies himself as both a servant (ʿabd) and a messenger (rasūl). The Qur’an similarly designates Muhammad as a servant (ʿabd) in verses such as Q. 8:41, Q. 17:1, Q. 18:1, and Q. 39:36, and as a messenger (rasūl) more than 200 times. 

In the concluding section of the text, it mentions, “Bless their measures and their weights (mudd and sāʿ) as you blessed the people of Mecca and bestow upon them two-fold blessings.” The phrase “their measures and their weights” (mudd and sāʿ) permeates the request with intensity and establishes a connection to Mecca, thus signifying the sanctification process of Mecca. However, if it is viewed from an alternative perspective, it creates the impression that the Prophet is invoking God to confer upon Medina a sanctuary status akin to that of Mecca. In essence, he seemingly initiates the sanctification process for Medina, which remains incomplete at this juncture. However, it is also probable that the phrase “I invoke blessings for the people of Medina” (adʿūka li-ahli al-Madīna an tabāraka) is paraphrased and intended to give the meaning of the sanctification as the context suggests. Another evidence supporting this possibility is that, in the Qur’an, Mecca is referred to with the word “blessing” (al-baraka). Therefore, the use of “al-baraka” is most likely interpolated instead of ḥarama or its derivatives by one of the transmitters due to mixing up the terminology.

V2 does not mention the location of the episode; therefore, there is no mention of al-Suqyā. It also does not mention the elements of “measures and their weights” mudd and sā. Furthermore, the text introduces new elements which were not mentioned in the first text, such as the boundaries of the consecrated area in Medina: “its two lava fields…”, “the shade of ʿAyr and the shade of Wuʿayr” and “It is a barīdun.” The two lava fields refer to Ḥarra Rahāṭ to the south and Ḥarra Khaybar to the north. Locally, these two fields are called al-ʿAyr and Kishb. These lava fields stand out due to their dark, rugged, and elongated terrain. ʿAyr and Wuʿayr refer to the two distinct hills, and barīd refers to the city boundaries known to people. Moreover, it specifies the prohibited actions in Medina, emphasising, “Its tree is not to be cut.” It is also highlighted that hunting is restricted in Medina, and this restriction differs from Mecca. The variation may be associated with additional limitations imposed during Hajj. 

Here is the complete text of V2:

> Abū ʿAbdillāh said: “The Messenger of God said: ‘Mecca is God’s sanctuary (ḥaram) that Abraham sanctified (ḥarrama) it. Medina is my sanctuary (ḥaramī), and what is between its two lava fields is a sanctuary (ḥaramun). Its tree is not to be cut. It is between the shade of ʿĀyir and the shade of Wuʿayr. Its prey is different from Mecca; some are eaten, and some are not eaten. It is a barīdun.’”<cite id="141n5"><a href="#zotero%7C167992%2FVETMPFAK">(M. b. Y. b. I. al-Kulaynī, 1986)</a></cite> (Bāb Taḥrīm al-Madīna).

As mentioned above, another noticeable difference is that instead of the “blessing” (al-baraka), which was used in V1, this variant uses another Quranic term to refer to the Kaʿba, the sanctuary “Your Sacred House” (baytika l-muḥarram) (Q. 14:37). Aside from these differences, there are some thematic similarities between the texts: “Mecca is God’s sanctuary (ḥaram) that Abraham sanctified (ḥarrama) it. Medina is my sanctuary (ḥaramī).” There is a clear comparison of the sanctuary status of Mecca and Medina, and between Abraham and Muhammad. However, the presence of numerous distinct elements in both texts raises the question of how such variations have occurred. As Harald Motzki articulated well in a natural transmission, there should be textual variations, paraphrasing, omissions and interpolations <cite id="v37w1"><a href="#zotero%7C167992%2FXVZVIRFZ">(Harald Motzki et al., 2011)</a></cite>.  This premise is based on the idea that reports handed down from generation to generation are bound to change. Fred Donner refers to this process as “compression” and “expansion,”  but the overall meaning remains consistent, which is the comparison of the Prophet’s contestation of Medina with Abraham’s consecration of Mecca <cite id="c6mjn"><a href="#zotero%7C167992%2FVETIM69M">(Donner, 1998)</a></cite>. Furthermore, as Orhan Elmaz, in his pertinent study, has shown, even within canonical hadith collections, linguistic and lexical variation reflects a process of textual adaptation and standardisation in early Islamic transmission <cite id="nn489"><a href="#zotero%7C167992%2FRM5T3QX9">(Elmaz, 2021)</a></cite>.  Similarly, Ahmed El Shamsy’s detailed analysis of the multiple recensions of Mālik’s Muwaṭṭaʾ demonstrates how early texts naturally developed variant readings through paraphrasing, omission, and reordering, while maintaining their essential content and intent <cite id="ni9gh"><a href="#zotero%7C167992%2FSMDM2A7L">(Shamsy, 2021)</a></cite>.  

The text of V3 and its chain of transmission are identical copies found in al-Kulaynī’s Kāfī. Therefore, it is not possible to date the unique elements found in these two variants, such as the delineation of the sanctuary’s boundaries and the specification of the prohibited acts, to a period earlier than al-Kulaynī. The text of V4  is likewise transmitted by al-Ṭūsī, though in a different work and with an alternate chain of transmission. The text reflects on the variations in the chain of transmission with various elements. First, Jaʿfar does not mention the Prophet’s name in this variant. The narrator ʿĀṣim claims to have heard Jaʿfar saying: “Mecca is the sanctity of Abraham and Medina is the sanctity of Muhammad, Kufa is the sanctity of ʿAlī b. Abī Ṭālib. ʿAlī sanctified Kufa, Abraham sanctified Mecca, and Muhammad sanctified Medina.”

The elements involving Abraham and Mecca, as well as their comparison to the Prophet and the sanctification of Medina, are shared with the earlier variants. Since these common elements existed in the previous versions, it may be possible to trace them back to Jaʿfar. However, a distinctive element differentiates the current variant: “Kufa is the sanctity of ʿAlī b. Abī Ṭālib. ʿAlī sanctified Kufa.” It appears that Jaʿfar or a subsequent narrator introduced this element into the text. It could be that drawing on his prior knowledge of the consecration practices of Abraham, the Prophet, and ʿAlī, Jaʿfar compressed these diverse accounts into a single narration for the sake of teaching in his study circles. In this report, he seems to be giving his opinion instead of directly quoting from the Prophet or ʿAlī, as he does not claim to report from either. Additionally, this supplementary element highlights the early Muslims’ perception of their operational area as sacred, especially considering Kufa as ʿAlī’s headquarters. It may project a later Shiʿi perspective rather than the transmission of Jaʿfar’s exact words. Yet, since it is absent from the earlier variants, further examination is warranted.

The text of V5 makes a significant contribution to the examination of textual variants. It again reports that ʿAlī stated that Medina was the sanctity of the Prophet, much like Mecca is the sanctuary of God. One can reasonably argue either that this omission of Abraham serves a stylistic or rhetorical purpose or that the text simply assumes readers’ familiarity with Abraham’s association with Mecca. In the case of Mecca, there is a reference to God, in Medina, a reference to the Messenger of God, and then to ʿAlī for Kufa (Makka ḥaram Allāh wa-l-Madīna ḥaram Rasūl Allāh and wa-l-Kūfa ḥaramī). In any case, the allusion to Abraham is implicit, as the construction of the Kaʿba is closely associated with him in the Qur’an.

The most striking part of V5 is that ʿAlī claimed that Kufa was his sanctuary:

> I heard Abū ʿAbdullāh [Imām Jaʿfar] saying: “ʿAlī b. Abī Ṭālib said, Mecca is the sanctuary of God, Medina is the sanctuary of the Messenger of God, and Kufa is my sanctuary. No tyrant can violate it without God breaking him. <cite id="qa3wl"><a href="#zotero%7C167992%2F6WD4V9CN">(M. b. Y. b. I. al-Kulaynī, 1986)</a></cite> (Bāb Taḥrīm al-Madīna).

It is important to note that the wording in this text reaffirms Kufa’s sanctuary status, which differs from the previous version. In this text, the previous text phrases it as “Kufa is my sanctuary” (al-Kūfa ḥāramī), which is first-person narration. In contrast, in the previous text, it is stated that “Kufa is the sanctity of ʿAlī b. Abī Ṭālib. ʿAlī sanctified Kufa” (al-Kūfa ḥaram ʿAlī b. Abī Ṭālib. Inna ʿAlī ḥarrama min al-Kūfa.), employing a third-person narration. This is crucial evidence for paraphrasing that aligns with the variations in transmission lines.

Furthermore, this variant ostensibly supports the findings in the previous variant that, as a PCL, Jaʿfar had access to these narratives, and he thus combined the material to teach his followers or students. Therefore, while potentially some of the elements that he narrated can be dated back to ʿAlī, some can be dated back to Jaʿfar only. 

The text of V6 is similar to the previous variant, but the last section of the two variants differs slightly. In al-Kāfī, the last section reads “Kufa is my sanctuary. No tyrant [can violate it] with an incident, without God breaking him” (al-Kūfa ḥaramī lā yurīduhā jabbārun bi-ḥādithatin illā qasamahu Allāh) while in Tadhīb al-aḥkām it is recorded as “Kufa is my sanctuary. No tyrant can commit a violation in it, without God breaking him” (al-Kūfa ḥaramī lā yurīduhā jabbārun yajūru fīhi illā qasamahu Allāh). 

The slight incongruities in the texts of these separate chains concur with the expected variations in oral transmission. Consequently, we may substantiate the assertion that Jaʿfar likely uttered statements regarding ʿAlī’s consecration of Kufa. The transmission of this information through two distinct chains until al-Ṭūsī allows us to date it to the era of Jaʿfar, who died in 148/765. As no transmitters are specified between Jaʿfar al-Ṣādiq and ʿAlī, we cannot be sure that he obtained these reports directly from his great-grandfather, ʿAlī. Furthermore, the elements, such as Abraham’s consecration of Mecca and the Prophet’s statement of comparison of himself to Abraham and Mecca and Medina, can be provisionally dated back to ʿAlī’s date of death, 40/661. This is plausible because the variants share common elements likely originating from the original report uttered by ʿAlī. Especially, Variant 1 is crucial; it has an uninterrupted Sunni transmission tracing back to ʿAlī and lends credibility to similar texts in Shiʿi sources. 


<!-- #endregion -->

<!-- #region citation-manager={"citations": {"r08by": [{"id": "167992/7859R5XF", "source": "zotero"}], "zxnqx": [{"id": "167992/3TT8KK3D", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} -->
### The Abū Hurayra Cluster
#### Isnād analysis:
Two variants were distributed through Abū Hurayra. One is documented in ʿAbd al-Razzāq’s Muṣannaf <cite id="zxnqx"><a href="#zotero%7C167992%2F3TT8KK3D">(al-Ṣanaʿānī, 1983)</a></cite> (Hadith no: 17149), and the other appears in Ibn Mājah’s Sunan <cite id="r08by"><a href="#zotero%7C167992%2F7859R5XF">(Ibn Māja, 1954)</a></cite> (Hadith no: 3113). These two variants were passed down through two separate chains of narration and recorded in different works. However, one of the chains reconnects with a chain from the ʿAlī b. Abī Ṭālib’s cluster at Saʿīd b. Abī Saʿīd al-Maqburī. Therefore, it is possible to detect similarities between these two variants in case the transmitter tampered with the texts. 

Variant 7 (V7) was recorded in ʿAbd al-Razzāq’s (d. 211/826) Muṣannaf, who heard the report from various transmitters up to Abū Hurayra (d. 59/681, Medina), who had heard it directly from the Prophet. Variant 8 (V8) was documented in Ibn Mājah’s (d. 273/887) Sunan  , who heard it from Abū Hurayra with a complete chain of transmission. 

<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} -->
#### Textual analysis:
V7 and V8 display a high degree of similarity, except for one element in the text of V7, which seemingly provides a context to the narrative: “The Prophet went out until he reached al-Suqyā of al-Ḥarra and said”. This element is not mentioned in V8, but interestingly, it is mentioned in V1. However, unlike V1, V7 does not include the elements of ablution and the existence of Saʿd b. Abī Waqqāṣ in the episode. It is conceivable that Saʿīd al-Maqburī introduced this only element into the texts. This is because both texts converge on him (see Table 1), and these elements are mentioned only in these two texts, which raises the possibility of textual contamination through Saʿīd al-Maqburī, who might have sought to provide context. At any rate, at this point, these elements can only be dated back to Saʿīd al-Maqburī’s date of death, 125/742-743. 

Moreover, there are also notable differences, notwithstanding the textual resemblance between the two variants examined in this cluster. In V7, the comparison element was stated as “O God, indeed Abraham, is your slave and messenger, he sanctified Mecca…as Abraham sanctified Mecca”, while in V8, it was stated as “O God! Abraham was your Prophet and friend. You sanctified Mecca with the words of Abraham.” In the comparison narrative, Abraham was referred to as “slave and messenger” in V7 and as “Prophet (nabī) and friend” in V8. This usage is again similar to the variant one, as it referred to Abraham as a “slave and friend”. Initially, it seems, using this terminology is another sign of textual corruption or contamination, possibly again introduced by Saʿīd al-Maqburī. However, a separate chain of transmission (V8) documented in Ibn Mājah’s Sunan weakens this possibility. On the other hand, the use of “slave and messenger” in both variants transmitted through Saʿīd al-Maqburī might potentially reinforce our suggestion of textual contamination by Saʿīd al-Maqburī. 

Interestingly, while in V1, “slave and messenger” refers to Abraham, in V7, it refers to Muhammad. This might indicate that the contamination is not intentional but rather a consequence of narrating variants of the same Hadith, suggesting a natural occurrence of mix-up. However, a similar element in a separately transmitted chain requires further explanation. It is possible that somebody else, such as al-ʿAlāʾ b. ʿAbd al-Raḥman, who had a mixed reputation, interpolated the element of “Prophet (nabī) and friend” in reference to Abraham, or it is the work of Abū Hurayra. As there is no compelling evidence against al-ʿAlāʾ, it is possible that Abū Hurayra introduced this element into the text to align and embellish the text with Quranic terminology, especially considering that these elements are traceable to him through two independent chains. 

Be that as it may, the elements of Abraham’s consecration of Mecca and the Prophet’s comparison of Mecca with Medina, as well as the boundaries encompassing the two lava fields, are commonly shared in both V7 and V8. Hence, there is a textual affinity between the clusters attributed to ʿAlī and those attributed to Abū Hurayra.
<!-- #endregion -->

<!-- #region citation-manager={"citations": {"2alu7": [{"id": "167992/DUKWBJJM", "source": "zotero"}], "qifg2": [{"id": "167992/K6XYNART", "source": "zotero"}], "w8aw9": [{"id": "167992/2QDX8ENH", "source": "zotero"}], "zcd86": [{"id": "167992/XVZVIRFZ", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} -->
### The ʿAmr b. Abī ʿAmr Cluster
#### Isnād analysis:
Variant 9 (V9) was documented in Muslim’s Ṣaḥīḥ,<cite id="2alu7"><a href="#zotero%7C167992%2FDUKWBJJM">(Muslim b. al-Ḥajjāj al-Qushayrī, 1991)</a></cite> from whom several transmitters reported it from Anas b. Mālik (d. 93/712, Medina and Basra), who was a Companion of the Prophet . There seems to be no apparent problem in this chain of transmission.

Variant 10 (V10) is found in Mālik b. Anas’s Muwaṭṭaʾ.<cite id="w8aw9"><a href="#zotero%7C167992%2F2QDX8ENH">(Ibn Anas, 1994)</a></cite> (Hadith no: 1645).  Yaḥyā al-Laythī (d. 234/848–9 or 236/850–1, al-Andalus and Medina) was one of the editors of Muwaṭṭaʾ.<cite id="zcd86"><a href="#zotero%7C167992%2FXVZVIRFZ">(Harald Motzki et al., 2011)</a></cite>  Mālik (d. 179/796, Medina) heard this variant from ʿAmr b. Abī ʿAmr (d.136/754, Medina), who was one of the main informants of the Mālik. Similar to the other chains in this cluster, ʿAmr heard it from Anas b. Mālik.

Variant 11 (V11) was documented in Tirmidhī’s (d. 279/892) Sunan.<cite id="qifg2"><a href="#zotero%7C167992%2FK6XYNART">(al-Tirmidhī, No date)</a></cite> (Hadith no: 3914) He heard the variant from Qutayba b. Saʿīd, who heard it from Mālik. Tirmidhī’s source is al-Anṣārī (d. 244/858-59, Medina, Samarra and Nishapur). He heard the variant from Maʿn [b. ʿĪsā] (d. 198/814, Medina), who widely reported from his teacher Mālik. According to the chain of transmission analysis, this report can be dated back to Mālik.
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} -->
#### Textual analysis:
The text of V9 includes a prelude about Anas b. Mālik and how he ended up being a servant of the Prophet. Upon this detail, he narrates how the Prophet sanctified Medina. The short narration begins with a detail that was not seen previously: the element of Mount Uhud. According to the text, the episode of the Prophet’s sanctification of Medina occurred when the Prophet approached Mount Uhud, which is 5 km North of Medina, and uttered a praise for the mount. He then uttered that he sanctified Medina as Abraham sanctified Mecca. After his speech, the Prophet prays for the blessings of God upon the people of Medina. According to the first variant of the ʿAlī b. Abī Ṭālib cluster, this episode took place in al-Suqyā, which is located to the southwest of the city. This could be a sign of a discrepancy or an error.

But who is accountable for introducing this element into the texts? Since the reference to Mount Uhud appears in all three variants within the ʿAmr b. Abī ʿAmr Cluster, there is a possibility that either he or his source, Anas b. Mālik introduced this anecdote into the text. However, technically, it can only be traced back to ʿAmr; therefore, he should be held responsible. The fact that he received mixed evaluations about his reliability in the biographical sources lends further support to this. The rest of the text, however, remains unchanged. These unchanged elements include the comparison between Abraham and Mecca to Muhammad and Medina, the delineation of two lava fields as the sanctified area, and the element of measurement, “O God! Bless (bāraka) the people of Medina in great measure”, included in V9. 

However, this element is absent in V10 and V11. Interestingly, the sole text containing the element of measurement and blessing (bāraka) is V1 of the ʿAlī b. Abī Ṭālib cluster. This could be attributed to both texts converging at Qutayba b. Saʿīd (see Table 1). At first glance, the textual evidence may suggest a textual conflation, in which elements from different reports were combined or interpolated. Especially, the motif of measurement (al-mudd wa-l-ṣāʿ) appears exclusively in those variants transmitted by Qutayba b. Saʿīd, which suggests that he may have merged or paraphrased related narratives. It is also likely that Qutayba was responsible for substituting ḥarrama (“sanctified”) with bāraka (“blessed”), since the term bāraka appears only in the versions connected to him: once alone (V1) and once together with ḥarrama (V9). Qutayba also transmits V11 and V12, where only ḥarama is preserved, though these versions are likely abridged, or it is possible that he transmitted them on different occasions, and that the substitution of ḥarrama with bāraka resulted from a lapse of memory. This pattern indicates that Qutayba’s recension may have introduced a slight rephrasing that merged the concepts of sanctification and blessing. This significant detail is crucial for dating the ʿAlī b. Abī Ṭālib cluster and strengthens the hypothesis that bāraka was mixed up with ḥarama in the transmission process.

As observed, the texts of V10 and V11 include the common elements mentioned earlier, with minimal paraphrasing. This similarity could stem from both texts being transmitted through Mālik, possibly in written form, thus explaining the lack of significant variations. Based on the available information, it is becoming evident that the core elements of the variants may be traced back to their CLs and, ultimately, to the Prophet. These elements are Abraham, Mecca, and their comparison to Muhammad and Medina.

<!-- #endregion -->

<!-- #region citation-manager={"citations": {"91t21": [{"id": "167992/9E7TZGQQ", "source": "zotero"}], "ak4yv": [{"id": "167992/DUKWBJJM", "source": "zotero"}], "i2v0q": [{"id": "167992/DUKWBJJM", "source": "zotero"}], "umavm": [{"id": "167992/DUKWBJJM", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} -->
### The Miscellaneous Variants
#### Isnād analysis
As noted above, four variants do not fit into any of the above clusters; therefore, they are to be studied in this section. The Variant 12 (V12) was documented in Muslim’s Ṣaḥīḥ.<cite id="ak4yv"><a href="#zotero%7C167992%2FDUKWBJJM">(Muslim b. al-Ḥajjāj al-Qushayrī, 1991)</a></cite> He heard the variant from Qutayba b. Saʿīd (d. 240/854-5), who also transmitted the first and the ninth variants. Qutayba b. Saʿīd heard it from a number of transmitters up to ʿAbdullāh b. Zayd b. ʿĀṣim, who was a companion of the Prophet from Medina. Although the transmitters in the chain are lesser known, there is a natural connection among them, as all were members of the Māzinī tribe.

Variant 13 (V13) was documented again in Muslim’s Ṣaḥīḥ <cite id="i2v0q"><a href="#zotero%7C167992%2FDUKWBJJM">(Muslim b. al-Ḥajjāj al-Qushayrī, 1991)</a></cite> who reported it from Jābir b. ʿAbdillāh (d. 78/697, Medina), a prominent Companion of the Prophet. Jābir taught at the Mosque of the Prophet until his death at the age of ninety-four. The variant was narrated by Jābir from the Prophet.

Similar to the previous two variants, Variant 14 (V14) is found in Muslim’s Ṣaḥīḥ <cite id="umavm"><a href="#zotero%7C167992%2FDUKWBJJM">(Muslim b. al-Ḥajjāj al-Qushayrī, 1991)</a></cite>, which reports it from Ibn Abī Saʿīd al-Khudrī (d. 112/730-31, Medina) <cite id="91t21"><a href="#zotero%7C167992%2F9E7TZGQQ">(Ibn Ḥibbān, 1973)</a></cite>, the son of a Companion of the Prophet, Abū Saʿīd al-Khudrī.  He heard this variant from his father Abū Saʿīd (d. circa 74/693, Medina), who was a prolific transmitter of the prophetic hadiths.  He heard this variant from the Prophet. Variant 15 (V15) is found in Ibn Abī Shayba’s Muṣannaf.  The chain of this variant is identical to the previous one, indicating that Muslim acquired this variant from Ibn Abī Shayba’s Muṣannaf. 

<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} -->
#### Textual analysis
The remaining four texts preserve the continuity of the comparison elements between Abraham’s sanctification of Mecca and Muhammad’s emulation of it in sanctifying Medina with a paraphrased rendition of these elements. The salient features of the paraphrasing, which can be examined in detail in Table 2 above, strongly suggest the likelihood of an oral transmission of these elements.

In addition to these elements, the mention of the “two lava fields” is present in V13 but absent in the remaining three variants. The inclusion of the two lava fields is noted in two variants of the ʿAlī b. Abī Ṭālib Cluster, all variants of the Abū Hurayra Cluster and the ʿAmr b. Abī ʿAmr Cluster. Hence, it is plausible that it was part of the original Hadith. The element of measurement is also included in the V12. This element was previously seen in V1 and V9 and is not present in the remaining variants. We connected this possibility to the transmission of these two variants by Qutayba b. Saʿīd. The existence of the element of measurement in the twelfth variant gives further credibility to this possibility, as he also transmits the twelfth variant. Therefore, it is certain that he interpolated this information into all the variants he transmitted except for the eleventh variant.

<!-- #endregion -->

<!-- #region citation-manager={"citations": {"79y85": [{"id": "167992/TM3VTF9V", "source": "zotero"}], "ioa0c": [{"id": "167992/NMT2HJ8V", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} -->
## AUGMENTING TRADITIONAL COLLATION: SEMANTIC SIMILARITY FOR TEXTUAL ANALYSIS
The transition from manual isnād-cum-matn analysis to computational validation is based on a crucial premise; semantic consistency across variant texts suggests the preservation of meaning within the transmission history. If independent transmission lines demonstrate high semantic similarity despite lexical divergence, this convergence strengthens the historical credibility of the shared core narrative. In this sense, LLM analysis does not replace philological reasoning; it quantifies it, thus allowing the traditional method’s interpretive understanding to be tested empirically across a broader textual field.

Analysis of textual variations is a fundamental, yet often tedious and time-consuming, task for scholars working with Hadith and other historical texts. Digital tools, such as CollateX,  have become widely adopted standards for automated lexical alignment. These tools are highly effective for comparing texts with close orthographic or word-for-word correspondence.<cite id="ioa0c"><a href="#zotero%7C167992%2FNMT2HJ8V">(Bordalejo &#38; Vázquez, 2021)</a></cite> 

However, the primary function of these lexical tools is to identify word-level differences. They encounter limitations when the scholarly goal is to rapidly assess whether passages that diverge in phrasing, syntax, or minor linguistic details still convey the same core conceptual content. 
Therefore, to complement traditional lexical analysis, this part of the study introduces an extension to the isnād-cum-matn analysis by way of integrating semantic similarity analysis. This computational method, executed via Large Language Models, addresses the historian’s challenge of filtering and evaluating meaning-preserving variants at scale. LLMs assess semantic similarity by converting text into high-dimensional numerical representations (embeddings) that capture meaning. Therefore, they enable the comparison of texts that diverge lexically yet convey the same conceptual content, allowing texts to be compared not only through their literal correspondences but also through their underlying meanings. This provides a complementary quantitative verification for the manual analysis, offering a more detailed and comprehensive examination of textual variants.

In this section, we first briefly present the current standard for automated multi-text alignment and then propose an alternative model that employs LLMs to analyse textual similarity with minimal preprocessing and without the need for specialised encoding. To illustrate the effectiveness of this model, we apply a multilingual model pretrained on Arabic texts. Although the model’s performance is imperfect, it demonstrates a significantly greater ability to group related texts based on semantic proximity, which can be challenging to achieve consistently using purely lexical collation methods. Moreover, the accuracy of this model can be further enhanced by fine-tuning the model on a domain-specific corpus, ideally using the SimCSE (Simple Contrastive Sentence Embedding) framework.<cite id="79y85"><a href="#zotero%7C167992%2FTM3VTF9V">(Gao et al., 2022)</a></cite>
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
### The Integrated Workflow: Combining Lexical and Semantic Analysis
Biagini et al.’s work demonstrates that automated lexical alignment, while highly effective at identifying surface-level textual differences, provides limited analytical results when variants lack orthographic similarity. To extend analysis beyond this surface-level comparison, our proposed workflow introduces Semantic Textual Similarity (STS) analysis following initial CollateX alignment. The LLM-based model assesses how closely each text aligns in meaning, identifying where variants converge or diverge conceptually rather than strictly orthographically. This semantic level provides essential context to help scholars distinguish genuinely related traditions from coincidental lexical overlaps or extensive paraphrases that standard collation alone may not clearly categorise. To illustrate this added analytical capacity, let us consider a traditional collation process and how our approach modifies it. We begin by assembling nine texts: four close variants, two distant variants, and three adversarial variants. The final category helps to show the need for semantic verification, as lexical collation struggles to effectively categorise highly divergent passages that nonetheless share conceptual content:
<!-- #endregion -->

```python editable=true slideshow={"slide_type": ""} tags=["hermeneutics"]
#import CollateX
from collatex import *

#import assessment functions
from script.supporting_code import input_df_to_collation_df, collation_to_df, combined_similarity_assessment
#import visualization functions
from script.supporting_code import plot_gap_freq_significance_scatter, plot_jaro_winkler_significance_scatter, plot_witness_similarity_matrix, plot_structural_similarity_heatmap, plot_witness_similarity_scatter, plot_similarity_levels_comparison, plot_semantic_clustering_dendrogram, plot_structural_dendrogram

#load language model
from sentence_transformers import SentenceTransformer, util
model = SentenceTransformer('intfloat/multilingual-e5-large')

# TqdmWarning and unexpected embeddings.position.ID may safely be ignored
```

<!-- #region editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
First it is worth recalling that CollateX aligns witnesses by characters. As such, it has no difficulty aligning texts of any language.
<!-- #endregion -->

```python editable=true jdh={"module": "object", "object": {"source": ["NO TITLE"]}} slideshow={"slide_type": ""} tags=["table-exCollation-*"]
collation = Collation()
collation.add_plain_witness("A", "The quick brown fox jumps over the dog")
collation.add_plain_witness("B", "The brown fox jumps over the lazy dog")

alignment_table = collate(collation, layout='vertical', segmentation=False)
print("English Table\n", alignment_table)

ar_collation = Collation()
ar_collation.add_plain_witness("A", "الثعلب البني السريع يقفز فوق الكلب")
ar_collation.add_plain_witness("B", "الثعلب البني يقفز فوق الكلب الكسول")
ar_alignment_table = collate(ar_collation, layout="vertical", segmentation=False)
print("Arabic Table\n", ar_alignment_table)
```

<!-- #region editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
Our close, distant, and adversarial transmissions are created as follows: witnesses A, B, C, and D are the core set of texts; witnesses E, F, and G are meant to confuse CollateX and dilute our collation; witnesses H and I are semantically similar texts that character based collation typically struggles with.
<!-- #endregion -->

```python editable=true jdh={"module": "object", "object": {"source": ["English Example"]}} slideshow={"slide_type": ""} tags=["hermeneutics", "table-engCollation-*"]
#@title English Example
#Close Transmission
en_collation = Collation()
en_collation.add_plain_witness("A", "The quick brown fox jumps over the dog")
en_collation.add_plain_witness("B", "The brown fox jumps over the lazy dog")
en_collation.add_plain_witness("C", "The quick red fox jumps over the tired dog")
# en_collation.add_plain_witness("C", "The quick red fox leaps over the tired dog")
en_collation.add_plain_witness("D", "The brown fox quickly jumps over the old dog")

#Adversarial Transmission
en_collation.add_plain_witness("E", "A purple elephant quickly dances under the old moonlight")
en_collation.add_plain_witness("F", "The ancient brown castle crumbled into the lazy sea below")
en_collation.add_plain_witness("G", "Seven quick robots assembled a spaceship over the garage")

# en_collation.add_plain_witness("E", "The purple elephant quickly dances over the old moonlight")
# en_collation.add_plain_witness("F", "The moving brown castle lept into the lazy sea below")
# en_collation.add_plain_witness("G", "Seven quick robots assembled a spaceship over the old garage")


# Distant Transmission
en_collation.add_plain_witness("H", "my drowsy mutt lies on the ground and a fast red vulpes bounds over it")
en_collation.add_plain_witness("I", "my snoozing hound doesn't lie on the ground and a fast red fox doesn't bound over it")

en_alignment_table = collate(en_collation, layout="vertical")
print(en_alignment_table)
```

```python editable=true slideshow={"slide_type": ""} tags=["hermeneutics"]
#@title assessment(collation), visualization(results)
def assessment(collation):
    df = collation_to_df(collation)
    return combined_similarity_assessment(df, model), df

def visualization(results, df):
    # 1.
    plot_gap_freq_significance_scatter(results, label_all=False)
    plot_jaro_winkler_significance_scatter(results, label_all=False)
    

    # 2.
    # variants = plot_focused_variant_significance(results)

    # 3.
    similarity_matrix = plot_witness_similarity_matrix(results)

    #3.2
    plot_structural_similarity_heatmap(results)

    # 4
    # Primary relationship visualization
    relationship_data = plot_witness_similarity_scatter(results)

    # 6
    # Multi-level comparison
    level_comparison = plot_similarity_levels_comparison(results)

    # 7
    # Hierarchical clustering
    dendrogram = plot_semantic_clustering_dendrogram(results, threshold=0.5)
    # 8
    linkage, dist_mat, witnesses = plot_structural_dendrogram(results, df, threshold=0.5)

```

```python editable=true slideshow={"slide_type": ""}
#run main assessment algorithms
en_results, en_df = assessment(en_collation)
```

<!-- #region citation-manager={"citations": {"6mr6a": [{"id": "167992/Z6674EUW", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
If we are to aggregate what agrees and disagrees between each text, we can visualise that relationship as a heatmap of lexical divergence. Examining this output the previous two figures, we are indeed able to see that variants A, B, C, and D form a core text. However, based purely on letter-level alignment, we are completely unable to determine that H and I are distant variants of that core while E, F, and G are conceptually unrelated texts. One might now incline towards analysing grammatical relationships; such modelling is often done through Universal Dependencies, which is a massive multilingual community framework for annotating grammatical relationships.<cite id="6mr6a"><a href="#zotero%7C167992%2FZ6674EUW">(de Marneffe et al., 2021)</a></cite>  Despite its usefulness, this framework remains a work in progress and lacks sufficient data coverage for many languages. This is precisely where the new generation of AI tools excels. By way of leveraging STS, we can quantify the conceptual distance between our texts. And once we combine that analysis with collation itself, we can produce an improved, semantically aware analysis.
<!-- #endregion -->

```python editable=true slideshow={"slide_type": ""} tags=["hermeneutics", "figure-engStructMatrix-*"]
plot_structural_similarity_heatmap(en_results)
```

<!-- #region editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
Passing the sentences through an LLM, we can generate a semantic similarity heatmap and a similarity tree. These outputs confirm that variants A, B, C, and D form a close tradition, with H and I being distantly related to them, while E, F, and G are all related neither to the core texts nor to each other.
<!-- #endregion -->

```python editable=true slideshow={"slide_type": ""} tags=["figure-engSTSMatrix-*", "hermeneutics"]
_ = plot_witness_similarity_matrix(en_results)
```

<!-- #region citation-manager={"citations": {"c4qmg": [{"id": "167992/2PC3DKDY", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
Furthermore, through analysing the collation alongside the semantic data, we can precisely see where our semantic differences affect the text. We are then able to quickly model a similarity tree from our results, which offers scholars an insight into the distance between every text and allows them to make an informed decision on where to begin their analysis.

Hence, we propose an integrated model that effectively combines the strengths of lexical alignment and semantic analysis, yielding robust and consistent results even when dealing with highly variable texts. We therefore reinforce collation with STS to produce multiple levels of analysis, allowing us to process both perfectly and imperfectly collated texts. For well-collated texts, the use of an LLM significantly aids the collation process by enabling precise meaning analysis at a large scale. Although off-the-shelf models are imperfect, <cite id="c4qmg"><a href="#zotero%7C167992%2F2PC3DKDY">(Biagini et al., 2023)</a></cite> they already perform well on Arabic language data and can be fine-tuned through the SimCSE framework for even greater precision. Crucially, this workflow remains lightweight and reproducible, thus it allows researchers to conduct meaning-based collation on ordinary computers without specialised infrastructure.
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
### Methodological Background: How Language Models Encode Meaning?
<!-- #endregion -->

<!-- #region citation-manager={"citations": {"3vrd1": [{"id": "167992/99LZUX7J", "source": "zotero"}], "pl8hx": [{"id": "167992/ZRNIKFUI", "source": "zotero"}], "sraa8": [{"id": "167992/HUMCFE4N", "source": "zotero"}], "vrspv": [{"id": "167992/7GWKJ6GM", "source": "zotero"}], "wlp8u": [{"id": "167992/VN9EYQ7L", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
### Sentence Transformers as Meaning Encoders; Embeddings as Semantic Coordinates
LLMs represent meaning geometrically, mapping words and sentences as vectors within a high-dimensional space. <cite id="wlp8u"><a href="#zotero%7C167992%2FVN9EYQ7L">(Mikolov et al., 2013)</a></cite> In this vector space, items that convey similar ideas occupy neighbouring positions. <cite id="vrspv"><a href="#zotero%7C167992%2F7GWKJ6GM">(Pennington et al., 2014)</a></cite>  These models do not understand language as humans do; rather, they encode statistical regularities that correspond closely to human intuitions of similarity.<cite id="3vrd1"><a href="#zotero%7C167992%2F99LZUX7J">(Nie et al., 2025)</a></cite> Such a geometric structure allows researchers to measure semantic proximity mathematically and to detect rephrasing or conceptual overlap across large textual corpora.<cite id="pl8hx"><a href="#zotero%7C167992%2FZRNIKFUI">(Vylomova et al., 2016)</a></cite> 

For our analysis, we will employ Sentence-BERT, a transformer model that produces fixed-length embeddings for sentences and enables comparison at a conceptual rather than lexical level.<cite id="sraa8"><a href="#zotero%7C167992%2FHUMCFE4N">(Reimers &#38; Gurevych, 2019)</a></cite> Sentence embeddings provide a useful quantitative method to highlight semantic proximity.
<!-- #endregion -->

<!-- #region citation-manager={"citations": {"zyank": [{"id": "167992/99LZUX7J", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
#### Measuring semantic distance: Cosine similarity explained
The reason we have chosen cosine similarity over other metrics is that it is a standard measure of textual similarity in embedding-based analysis.<cite id="zyank"><a href="#zotero%7C167992%2F99LZUX7J">(Nie et al., 2025)</a></cite> Cosine similarity quantifies the angular distance between two vectors in high-dimensional space, regardless of their magnitude. In practice, this means it measures how similarly two texts are oriented in this “semantic space”. This makes it especially well-suited for comparing mixed textual units (e.g., short phrases vs. full sentences).  Consequently, cosine similarity has emerged as the de facto standard for evaluating sentence embeddings in tasks such as STS, as it combines mathematical rigour with interpretive transparency, which makes it well-suited to humanistic research.

<!-- #endregion -->

<!-- #region citation-manager={"citations": {"0l2ps": [{"id": "167992/XBUBN9YB", "source": "zotero"}], "2etyh": [{"id": "167992/J5LB4B82", "source": "zotero"}], "ey687": [{"id": "167992/7UBWXN9I", "source": "zotero"}], "fnn9x": [{"id": "167992/WQNAPATM", "source": "zotero"}], "n20ou": [{"id": "167992/XBUBN9YB", "source": "zotero"}], "nh2hw": [{"id": "167992/42F7HEVV", "source": "zotero"}], "ulzfr": [{"id": "167992/DWF9N3UN", "source": "zotero"}], "vl30o": [{"id": "167992/XBUBN9YB", "source": "zotero"}], "w7oug": [{"id": "167992/IHEIIWZR", "source": "zotero"}], "yaie5": [{"id": "167992/7LTBFGEP", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
#### Model Selection
We considered multiple models, the top-performing model from the Massive Text Embedding Benchmark (MTEB) <cite id="ey687"><a href="#zotero%7C167992%2F7UBWXN9I">(Enevoldsen et al., 2025)</a></cite> for STS tasks, but ultimately chose Microsoft’s Multilingual-E5-Large model. <cite id="yaie5"><a href="#zotero%7C167992%2F7LTBFGEP">(Wang et al., 2024)</a></cite> The other models were either paid, <cite id="2etyh"><a href="#zotero%7C167992%2FJ5LB4B82">(Comanici et al., 2025)</a></cite> far too large to easily use for personal computers, <cite id="w7oug"><a href="#zotero%7C167992%2FIHEIIWZR">(Zhang et al., 2025)</a></cite> or not designed for Arabic. <cite id="nh2hw"><a href="#zotero%7C167992%2F42F7HEVV">(Thakur et al., 2021)</a></cite> There is a lack of multilingual studies standards assessing the abilities of LLMs within each given language. <cite id="n20ou"><a href="#zotero%7C167992%2FXBUBN9YB">(Abdelazim et al., 2023)</a></cite> Fortunately, a recent study demonstrated that the base E5  model<cite id="fnn9x"><a href="#zotero%7C167992%2FWQNAPATM">(Wang, Yang, Huang, Jiao, et al., 2024)</a></cite> not only proved effective in embedding Arabic texts, but it “outperformed all other models” in STS tasks. <cite id="vl30o"><a href="#zotero%7C167992%2FXBUBN9YB">(Abdelazim et al., 2023)</a></cite> Moreover, since the release of the analysis of the base E5 model, an open source multilingual version of E5 has been released and has achieved the 8th-best model in STS rankings on the MTEB leaderboard.  Thus, we have chosen this model not only for the family’s proven results on Arabic texts, but also because Multilingual-E5-Large is able to run on average computers easily.

To demonstrate that this model can understand Arabic, we tested how accurately Multilingual-E5-Large can differentiate the meaning of different phrases in Arabic via a standard dataset that the model was not trained on. <cite id="ulzfr"><a href="#zotero%7C167992%2FDWF9N3UN">(Nacar &#38; Koubaa, 2024)</a></cite> This is a Natural Language Inference (NLI) dataset which contains a premise sentence and three hypothesis sentences, each of which either agreed (entailment), disagreed (contradiction), or did not expressly go against or concur with the original sentence (neutral).<cite id="0l2ps"><a href="#zotero%7C167992%2FXBUBN9YB">(Abdelazim et al., 2023)</a></cite>
<!-- #endregion -->

```python editable=true slideshow={"slide_type": ""} tags=["hermeneutics", "figure-nliHeatmap-*"]
from IPython.display import Image 
metadata={
    "jdh": {
        "module": "object",
        "object": {
            "type":"image",
            "source": [
                "NLI Labels categorization demonstration"
            ]
        }
    }
}
display(Image("media/Picture 7.1.png"), metadata=metadata)
```

<!-- #region editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
We visualised the scores as a colour-coded histogram to provide a more intuitive sense of how the model maps semantic similarity and divergence between sentences. Three general regions emerge for each hypothesis. When entailment and contradiction are compared directly, the distinction becomes even more pronounced.
<!-- #endregion -->

```python editable=true slideshow={"slide_type": ""} tags=["hermeneutics", "figure-nliDistribution3-*"]
from IPython.display import Image 
metadata={
    "jdh": {
        "module": "object",
        "object": {
            "type":"image",
            "source": [
                "Distriution Comparision: Entailment vs Neutral vs Contradiction"
            ]
        }
    }
}
display(Image("media/Picture 7.2.png"), metadata=metadata)
```

```python editable=true slideshow={"slide_type": ""} tags=["hermeneutics", "figure-nliDistribution2-*"]
from IPython.display import Image 
metadata={
    "jdh": {
        "module": "object",
        "object": {
            "type":"image",
            "source": [
                "Distriution Comparision: Entailment vs Contradiction"
            ]
        }
    }
}
display(Image("media/Picture 7.3.png"), metadata=metadata)
```

<!-- #region citation-manager={"citations": {"5pcxc": [{"id": "167992/TM3VTF9V", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
But are these categories actually meaningful? Do they reflect the model’s genuine ability to distinguish Arabic texts by their meaning? The answer is yes. We have also conducted several statistical tests to examine the underlying reasons, which are discussed below. It may at first appear that there is substantial overlap; however, the distribution of these regions reveals a graded semantic structure within the cosine similarity space, precisely what one would expect from a well-calibrated semantic model. The results display a clear monotonic ordering: Entailment → Neutral → Contradiction, with statistically significant differences between all categories. In plain terms, the model consistently distinguishes sentence pairs as highly concordant, clearly contradictory, or genuinely neutral. Nevertheless, the effect sizes add nuance to this overall picture. The largest effect size gap remains between entailment and contradiction, confirming a strong ability to differentiate the meanings of sentences at the extremes. We also are able to identify 0.7 as the threshold, above which values are overwhelmingly entailment; this will inform our analysis. Lastly, the gradual effect size shrinkage suggests that the model struggles to consistently distinguish unrelated or topic-matching neutral statements from outright contradictions based on similarity alone, highlighting the limits of surface-level semantic similarity.

Nonetheless, since the model orders the three NLI categories by semantic proximity, these results demonstrate that cosine similarity recovers a meaningful semantic continuum in Arabic text, even in zero-shot contexts. This pattern supports using similarity thresholds for coarse filtering, while acknowledging that neutral cases may require additional reasoning beyond embedding similarity. To enhance the model’s capacity to capture finer-grained semantic distinctions, particularly between neutral and contradictory relationships, the SimCSE <cite id="5pcxc"><a href="#zotero%7C167992%2FTM3VTF9V">(Gao et al., 2022)</a></cite> framework offers the most effective and promising avenue for future research.
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
#### The Semantically Aware Collation Pipeline 
<!-- #endregion -->

<!-- #region citation-manager={"citations": {"6x6gw": [{"id": "167992/6QISGCPU", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
##### Data Preparation and Preprocessing
Data preparation is frequently one of the most time-consuming aspects of digital analysis projects of this kind. That is why we aimed to automate as much of the preprocessing as possible. We have created a publicly available notebook that can accept a list of Arabic texts and will remove all non-Arabic characters, as well as numbers, punctuation, and vowel markings:

ٱلسَّلَامُ عَلَيْكُمْ	→	السلام عليكم

After cleaning the text, we pattern-matched and removed all honorifics, such as “عليه السلام”. Lastly, we separated the isnāds from the matn. Though tools to automatically separate these two parts of the text are in development, such as Ukhbert, <cite id="6x6gw"><a href="#zotero%7C167992%2F6QISGCPU">(Alkaoud &#38; Syed, 2021)</a></cite> we chose manual separation as it proved more reliable for our dataset.
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} -->
##### Traditional Collation Baseline
<!-- #endregion -->

```python editable=true jdh={"module": "object", "object": {"source": ["Original Hadiths as a Dataframe"]}} slideshow={"slide_type": ""} tags=["table-inputdf-*"]
#@title original hadiths as a dataframe
#this can be freely edited or replaced with any csv file

#column 1 is expected to be named 'variant' and consist of number entries
#column 2 can be freely renamed
import io
import pandas as pd
input_df = pd.read_csv(io.StringIO('''
variant,full hadith
1,قال خرجنا مع رسول الله حتى إذا كنا بحرة السقيا التي كانت لسعد بن أبي وقاص فقال رسول الله ائتوني بوضوء فتوضأ ثم قام فاستقبل القبلة ثم قال اللهم إن إبراهيم كان عبدك وخليلك ودعا لأهل مكة بالبركة وأنا عبدك ورسولك أدعوك لأهل المدينة أن تبارك لهم في مدهم وصاعهم مثلي ما باركت لأهل مكة مع البركة بركتين
2,قال رسول الله إن مكة حرم الله حرمها إبراهيم وإن المدينة حرمي ما بين لابتيها حرم لا يعضد شجرها
3,قال رسول الله ان مكة حرم الله حرمها ابراهيم وان المدينة حرمي ما بين لابتيها حرم لا يعضد شجرها وهو ما بين ظل عاير إلى ظل وعير وليس صيدها كصيد مكة يؤكل هذا ولا يؤكل ذاك وهو بريد
4,مكة حرم إبراهيم والمدينة حرم محمد والكوفة حرم علي بن أبي طالب إن عليا حرم من الكوفة ما حرم إبراهيم من مكة وما حرم محمد من المدينة
5,قال أمير المؤمنين مكة حرم الله والمدينة حرم رسول الله والكوفة حرمي لا يردها جبار يجور فيه إلا قصمه الله
6,قال أمير المؤمنين مكة حرم الله والمدينة حرم رسول الله والكوفة حرمي لا يريدها جبار بحادثة إلا قصمه الله
7,أن النبي خرج حتى إذا كان عند السقيا من الحرة قال اللهم إن إبراهيم عبدك ورسولك حرم مكة اللهم وإني أحرم ما بين لابتي المدينة مثل ما حرم إبراهيم مكة
8,أن النبي قال اللهم إن إبراهيم خليلك ونبيك وإنك حرمت مكة على لسان إبراهيم اللهم وأنا عبدك ونبيك وإني أحرم ما بين لابتيها
9,قال رسول الله لأبي طلحة التمس لي غلاما من غلمانكم يخدمني فخرج بي أبو طلحة يردفني وراءه فكنت أخدم رسول الله كلما نزل وقال في الحديث ثم أقبل حتى إذا بدا له أحد قال هذا جبل يحبنا ونحبه فلما أشرف على المدينة قال اللهم إني أحرم ما بين جبليها مثل ما حرم به إبراهيم مكة اللهم بارك لهم في مدهم وصاعهم
10,أن رسول الله طلع له أحد فقال هذا جبل يحبنا ونحبه اللهم إن إبراهيم حرم مكة وأنا أحرم ما بين لابتيها
11,أن رسول الله طلع له أحد فقال هذا جبل يحبنا ونحبه اللهم إن إبراهيم حرم مكة وإني أحرم ما بين لابتيها
12,أن رسول الله قال إن إبراهيم حرم مكة ودعا لأهلها وإني حرمت المدينة كما حرم إبراهيم مكة وإني دعوت في صاعها ومدها بمثلي ما دعا به إبراهيم لأهل مكة
13,قال النبي إن إبراهيم حرم مكة وإني حرمت المدينة ما بين لابتيها لا يقطع عضاهها ولا يصاد صيدها
14,أنه سمع رسول الله يقول إني حرمت ما بين لابتي المدينة كما حرم إبراهيم مكة
15,أنه سمع النبي يقول إني حرمت ما بين لابتي المدينة كما حرم إبراهيم مكة
'''), header=0, index_col=0)

input_df.head()
```

```python editable=true jdh={"module": "object", "object": {"source": ["Collation DataFrame"]}} slideshow={"slide_type": ""} tags=["hermeneutics", "table-collationdf-*"]
#convert texts to collation
collation_df = input_df_to_collation_df(input_df)
collation_df.head()
```

```python editable=true slideshow={"slide_type": ""} tags=["hermeneutics"]
#run main assessment algorithms
ar_results = combined_similarity_assessment(collation_df, model)
```

<!-- #region editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
We fed the normalised texts into CollateX using the default parameters of the software. We then analysed our data by computing the STS of aligned words, then phrases, then the full text. Each level of analysis gave insight into the set of variants.
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
#### Layered Semantic Analysis
To better understand our scoring, here are 10 entries from each level of analysis.
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
##### Micro-Semantic Similarity
Micro-semantic similarity refers to our word-level analysis. We calculated the internal score of each “token” column in Figure 9.1 by comparing each word, or aligned group of words, with one another. This metric is how we assess the coherence of the collation itself through average, minimum, and maximum similarity scores.
<!-- #endregion -->

```python editable=true jdh={"module": "object", "object": {"source": ["Micro-Semantic Similarity"]}} slideshow={"slide_type": ""} tags=["hermeneutics", "table-microSim-*"]
from script.supporting_code import micro_semantic_similarity
micro_df, _ = micro_semantic_similarity(collation_df, model, return_pairwise=True)

micro_df.head(10)
```

<!-- #region editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
###### Meso-Semantic Similarity

Meso-semantic scores capture phrase-level relationships. This metric aimed to determine whether adjacent collated words formed coherent phrases across variants. We defined a three-word sliding window for each phrase, aligning two texts side by side. For every phrase in Variant A, all possible corresponding phrases in Variant B were compared, skipping gaps, and their cosine similarities were calculated. The highest-scoring phrase pair was then selected and linked. This procedure was repeated for all variant pairs. The expectation was that this approach would identify instances of rephrasing while, by design, yielding slightly optimistic similarity estimates. The chosen window size was determined through iterative testing.
<!-- #endregion -->

```python editable=true jdh={"module": "object", "object": {"source": ["Meso-Semantic Similarity"]}} slideshow={"slide_type": ""} tags=["hermeneutics", "table-mesoSim-*"]
from script.supporting_code import meso_semantic_similarity
meso_df, _ = meso_semantic_similarity(collation_df, model)

meso_df.head(10)
```

<!-- #region editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
###### Macro-Semantic Similarity

Lastly, macro-semantic similarity refers to the cosine similarity of the full texts themselves. We concatenated all the texts to recreate the variants prior to collation and then computed the full semantic textual similarities. We have found these scores to be most reliable 
<!-- #endregion -->

```python editable=true jdh={"module": "object", "object": {"source": ["Macro-Semantic Similarity"]}} slideshow={"slide_type": ""} tags=["hermeneutics", "table-macroSim-*"]
from script.supporting_code import macro_semantic_similarity
macro_df = macro_semantic_similarity(collation_df, model)

macro_df.head(10)
```

<!-- #region citation-manager={"citations": {"et1qb": [{"id": "167992/HUMCFE4N", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
##### Similarity normalisation
Since the cosine similarity scores generated by the embedding model were naturally compressed within a narrow range (approximately 0.7–1.0), we applied a linear rescaling to normalise them to the interval [0, 1]. This transformation, which preserves the relative distances among data points, was adopted to improve interpretability and enable consistent comparison across variants. <cite id="et1qb"><a href="#zotero%7C167992%2FHUMCFE4N">(Reimers &#38; Gurevych, 2019)</a></cite> Scores below 0.7 were clipped to 0, and values marginally exceeding 1.0 were capped at 1.0. The transformation is defined as follows:
<!-- #endregion -->

<!-- #region tags=["hermeneutics"] -->
$$
s_{\text{norm}} = \frac{s - 0.7}{1.0 - 0.7}
$$
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} tags=["hermeneutics"] -->
_Where “s” is the raw cosine similarity and “s norm” is the normalised score._
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} -->
#### Results: Semantic Patterns in Hadith Transmission
##### Overall Semantic-Structural Landscape
<!-- #endregion -->

```python editable=true jdh={"module": "object", "object": {"source": ["Similarity Comparison Across Analysis Levels shows how granularity affects textual relationships"], "type": "image"}} slideshow={"slide_type": ""} tags=["figure-simHistogram-*"]
_ = plot_similarity_levels_comparison(ar_results)
```

<!-- #region editable=true slideshow={"slide_type": ""} -->
This bar chart presents a multi-level comparison of textual similarity across four distinct analytical dimensions: micro (token/column), meso (phrase), macro (full text), and structural (alignment-based). Each score represents the normalised average similarity across all variant pairs under analysis, enabling direct cross-comparison.

Crucially, while semantic scores (micro, meso, macro) reflect the degree of meaning preservation between texts, the structural score is derived from CollateX’s alignment algorithm, which measures how consistently tokens appear across columns. Thus, it reflects positional consistency, not semantic coherence. The results reveal a striking pattern. At the micro level, token-level similarity averages 0.661, indicating moderate lexical overlap across variants, which suggests that while word choice varies, core vocabulary remains largely consistent. At the meso level, phrase-level similarity drops significantly to 0.430, revealing that local syntactic groupings do not align well without broader contextual anchoring. This implies that rephrasing often occurs at the phrasal level, disrupting surface-level correspondence. At the macro level, full-text similarity rises sharply to 0.704, which is well above our established entailment threshold. This demonstrates that despite variation at lower granularities, the overall semantic structure of the texts remains highly coherent. In contrast, the structural score of 0.379, the lowest of all, indicates that CollateX struggled to produce consistent columnar alignments, which reflects substantial gaps or mismatches in positional correspondence. Thus, it suggests that human readers would likely perceive stronger structural cohesion than the algorithmic alignment captures.

These findings collectively support a key understanding: semantic fidelity is preserved most vigorously at the holistic, macro level; even as surface-level structures (columns, phrases) diverge significantly. This pattern is entirely consistent with established theories in Hadith studies we observed in Section 1, which posit that orally transmitted traditions prioritise meaning preservation over verbatim replication. Variants may exhibit substantial rephrasing, syntactic rearrangement, or omission and interpolation, yet the core narrative remains remarkably stable.
In essence, our computational analysis validates the perception long held by scholars of oral transmission, specifically, Hadith studies. What matters is not the exact wording, but the core cognitive framework. The low structural score further highlights the limitations of purely lexical collation tools like CollateX when the primary scholarly objective is meaning preservation across fluid, context-sensitive textual traditions. This convergence reinforces the need for semantically aware methods in digital humanities scholarship.
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} -->
###### Semantic-Structural Discrepencies - Gaps in Our Collation
<!-- #endregion -->

```python editable=true jdh={"module": "object", "object": {"source": ["NO TABLE TITLE/ FIGURE TITLE: Collation vs Micro Semantic Similiarity How well does CollateX group tokens by meaning? "]}} slideshow={"slide_type": ""} tags=["table-gapFreqTable-*", "figure-gapFreqPlot-*"]
gap_freq_df = plot_gap_freq_significance_scatter(ar_results, label_all=False, label_top=False).sort_values(by=["gap_freq"], ascending=False)
gap_freq_df.head(10)
```

<!-- #region editable=true slideshow={"slide_type": ""} -->
From this gap frequency alignment, we can see that most columns, as attested in <figure-inputdf-*></figure-inputdf-*>, have many gaps in them. Nonetheless, those words that do get aligned tend to be the same mostly. This is what we expect from a character alignment program. Some columns, such as column index 37, have correctly aligned the relative pronoun “ما” between almost every variant. However, as a whole, collation alone is not sufficient for recovering meaning just by character-level alignment. Moreover, the extremely low R^2 value of 0.0008 indicates that there is virtually no linear relationship between structural consistency and semantic similarity across variant columns. In practical terms, this means that regardless of whether columns are more or less consistently filled, they do not tend to contain semantically similar tokens. Whether a column appears frequently across witnesses (low gap frequency) or rarely (high gap frequency) offers almost no predictive power about the degree to which its tokens share meaning within collated groups. This finding suggests that the structural alignment produced by CollateX, while useful for positional comparison, does not inherently reflect semantic coherence; therefore, it emphasises the necessity of semantic verification in the analysis of fluid, orally transmitted Hadith data.
<!-- #endregion -->

<!-- #region citation-manager={"citations": {"budbw": [{"id": "22122298/TG9N5K8T", "source": "zotero"}], "js6nl": [{"id": "22122298/7ANRDQPP", "source": "zotero"}]}} editable=true slideshow={"slide_type": ""} -->
Now, let us swap our lens and look at these collations from within. We measure the texts using the same algorithm that CollateX used to create the collation, namely Jaro-Winkler, a standard distance and similarity metric introduced created by <cite id="js6nl"><a href="#zotero%7C22122298%2F7ANRDQPP">(Jaro, 1989)</a></cite> and <cite id="budbw"><a href="#zotero%7C22122298%2FTG9N5K8T">(Winkler, 1990)</a></cite>. We are able to see that collated similarity scores of the entries are roughly approximate to the LLM embeddings themselves, but there is a consistent pattern of the raw string comparision being over-confident on how similiar two texts are. 

For instance, when there is full textual agreement between the entries, both scores rank their similarity at 100, such as the nine columns clustered in the top right of the graph. However, in cases such as columns 8 and 9, where a mix of texts are collated, the Jaro-Winkler score is as much as 22 percentage points more confident than our LLM. For cases where we want to be sceptical of our texts, there is strong preference in using an LLM to measure textual similarity. Furthermore, we have limited our LLM by excluding context and comparing individual words and phrases. Including more nuanced text selections will further sperate gap between STS and Jaro-Winkler. Through this comparision, there is a strong argument to create our collation itself through STS so that we are able to group like phrases, and we never lose the ability to measure textual difference through letter based approaches as well.
<!-- #endregion -->

```python editable=true jdh={"module": "object", "object": {"source": ["NO TABLE TITLE/ FIGURE TITLE: CoolateX Text Similiarity vs Micro Semantic Similiarity How similar are CollateX groupings across Letters and meaning? "]}} slideshow={"slide_type": ""} tags=["table-jaroWinkTable-*", "figure-jaroWinkPlot-*"]
jaro_winkler_df = plot_jaro_winkler_significance_scatter(ar_results, label_all=False, label_top=False).sort_values(by=["struct_score"], ascending=True)
jaro_winkler_df.head(10)
```

<!-- #region editable=true slideshow={"slide_type": ""} -->
###### Semantic-Structural Discrepencies - Our Texts from a Birds Eye View
<!-- #endregion -->

```python editable=true jdh={"module": "object", "object": {"source": ["Semantic Similarity Matrix (Full Text STS from LLM)"], "type": "image"}} slideshow={"slide_type": ""} tags=["figure-arSTSMatrix-*"]
_ = plot_witness_similarity_matrix(ar_results)
```

```python editable=true jdh={"module": "object", "object": {"source": ["Structural Agreement Matrix (Token Identity from Collation)"], "type": "image"}} slideshow={"slide_type": ""} tags=["figure-arStructMatrix-*"]
#Table 9.4
plot_structural_similarity_heatmap(ar_results)
```

<!-- #region editable=true slideshow={"slide_type": ""} -->
 For a better comparison, we can translate these heatmaps to a dendrogram below:
<!-- #endregion -->

```python editable=true jdh={"module": "object", "object": {"source": ["Witness Clustering based on Macro-Semantic Similarity Lower branches= closer semnatically/ Witness Clustering by Structural Alignement Lower branches = closer collation"], "type": "image"}} slideshow={"slide_type": ""} tags=["figure-dendrograms-*"]
_ = plot_semantic_clustering_dendrogram(ar_results, threshold=0.5)
_ = plot_structural_dendrogram(ar_results, collation_df, threshold=0.5)
```

<!-- #region editable=true slideshow={"slide_type": ""} -->
We can already observe clear patterns in the results. Pairs such as V2–V3, V5–V6, V10–V11, and V14–V15 exhibit near-identical correspondence. Their close alignment is evident both in the character-level collation heatmap and in the semantic heatmap, which further shows how these pairs cluster with other narratively related texts. Conversely, variants V1, V4, V5, V6, and V9 share relatively little with the remaining traditions, particularly the pronounced dissimilarity between V1 and V4. Diagonal entries are ignored, as they represent self-comparison, and only similarity scores above 0.7 or below 0.3 are displayed for our identified thresholds of semantic relatedness.
<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} -->
##### Semantic Dendrogram:
If we recall <table-isnadtree-*></table-isnadtree-*>, we can see that our semantic tree rather closely reflects how one might group these variants if we focused on the identified narrative beats of the story. V4, V5, V6 mention ʿAlī and Kufa, and they are grouped together here. V10 and V11 are exact matches and thus have the highest semantic confidence score. V2, V3, and V13 are the only texts to mention that it is forbidden to cut down a tree, and they are grouped here. In this sense, we have reconstructed the thematic groupings of the Hadith, just through an unrefined model. 

Furthermore, if we compare with the clusters as identified with isnād-cum-matn analysis, we see our most confident pairs (V5 and V6, V10 and V11, V14 and V15) are all grouped by cluster. In this manner, we were able to approximate chains of transmission solely via textual comparison. However, our most interesting data are the variants that became associated across clusters: 
<!-- #endregion -->

```python editable=true slideshow={"slide_type": ""} tags=["figure-isnadtreeAbbr-*"]
from IPython.display import Image, display
metadata={
    "jdh": {
        "module": "object",
        "object": {
            "type":"image",
            "source": [
                "Abbreviated Isnad Tree"
            ]
        }
    }
}
display(Image("media/Picture 9.7.png"), metadata=metadata)
```

<!-- #region editable=true slideshow={"slide_type": ""} -->
#### 6. Case Studies: 
##### Case Study 1: 
We first examine the most confident pairs: V2–V3, V5–V6, V10–V11, and V14–V15. Each demonstrates near-identical wording, which explains their strong alignment. The collation process also linked these pairs at the word level, though minor rephrasing introduced slight uncertainty. The AI-generated graph, however, consistently grouped these texts together, which confirms their close similarity. These pairs thus function as important sanity checks and reinforce the reliability of the model. More interesting, however, is the model’s unexpected association of V4 with the almost identical pair V5–V6; traditional collation entirely failed to detect this link.Case Study 2: 
<!-- #endregion -->

##### Case Study 2:

```python editable=true slideshow={"slide_type": ""} tags=["table-v456Arabic-*"]
from IPython.display import Image 
metadata={
    "jdh": {
        "module": "object",
        "object": {
            "type":"image",
            "source": [
                "Variants 4, 5, 6 in Arabic"
            ]
        }
    }
}
display(Image("media/Text1.png"), metadata=metadata)
```

<!-- #region editable=true slideshow={"slide_type": ""} -->
These three traditions form half of the ʿAlī Ibn Abī Ṭālib cluster. We can see that V5 and V6 have nearly identical wording, while V4 is worded quite differently. All the variants mention ʿAlī and the city of Kufa, but V5 and V6 refer to ʿAlī by his title “Amīr al-Mu’minīn”, while V4 refers to him by his name. Furthermore, of the 15 total traditions, these are the only variants that mention ʿAlī or Kufa. Lastly, V4 has a completely different wording for the event. This distinction helps explain why CollateX did not detect their similarity and why our semantic scores lack confidence in the grouping. Yet, our semantic model did still correctly group these three texts.
<!-- #endregion -->

##### Case Study 3:

```python editable=true slideshow={"slide_type": ""} tags=["table-v19Arabic-*"]
from IPython.display import Image 
metadata={
    "jdh": {
        "module": "object",
        "object": {
            "type":"image",
            "source": [
                "Variants 1, 9 in Arabic"
            ]
        }
    }
}
display(Image("media/Text2.png"), metadata=metadata)
```

<!-- #region editable=true slideshow={"slide_type": ""} -->
In our final case study, we are presented with V1 and V9, both of which have long, unrelated preambles. If we are to remove the preambles and model the texts again, we have V9 correctly joining V10 and V11 to reconstruct the ʿAmr b. Abī ʿAmr cluster in its entirety. However, our model is still not confident in separating V1 from V9 and the ʿAmr b. Abī ʿAmr cluster.
<!-- #endregion -->

```python editable=true slideshow={"slide_type": ""} tags=["figure-procV1V9Dendrogram-*"]
from IPython.display import Image, display
metadata={
    "jdh": {
        "module": "object",
        "object": {
            "type":"image",
            "source": [
                "Processed V1-V9 Dendrogram"
            ]
        }
    }
}
display(Image("media/Picture 10.2.png"), metadata=metadata)
```

<!-- #region citation-manager={"citations": {"acg1s": [{"id": "167992/ENAMXBB7", "source": "zotero"}], "xexrt": [{"id": "167992/J24STZ3B", "source": "zotero"}]}} -->
If we are to examine the texts of V1 and V9, we note that they are the only variants employing the verb bāraka (“to bless”). Interestingly, at this point, the AI-based semantic analysis revealed a crucial textual relationship that we had previously overlooked in manual analysis. The model produced a high similarity score between (V1) and (V9), which prompted a re-examination of their wording. We subsequently identified a significant variation in the use of bāraka and ḥarrama. The AI model’s detection of semantic proximity between these variants enabled us to observe that bāraka appears only in versions linked to Qutayba; once alone (V1) and once alongside ḥarrama (V9). This finding makes it more likely that a slight paraphrasing occurred, merging the concepts of sanctification and blessing, with Qutayba plausibly responsible for this variation.

Furthermore, as seen below, indeed, when bāraka in V1 is replaced with ḥarama, the model generates an entirely different clustering pattern for that variant. This replacement is based on the Lexical Normalisation method, which allows for a direct comparison of the syntactic and semantic structures consistent with Noam Chomsky’s “minimalist framework” to analyse linguistic patterns. <cite id="acg1s"><a href="#zotero%7C167992%2FENAMXBB7">(Chomsky, 2001)</a></cite> This approach reduces the complexity of the problem by concentrating on a single, normalised form for comparison, rather than attempting to accommodate all possible lexical and syntactic variations at once.<cite id="xexrt"><a href="#zotero%7C167992%2FJ24STZ3B">(Niyogi, 2001)</a></cite> 
<!-- #endregion -->

```python editable=true slideshow={"slide_type": ""} tags=["figure-procV1V9DendrogramB2H-*"]
from IPython.display import Image, display
metadata={
    "jdh": {
        "module": "object",
        "object": {
            "type":"image",
            "source": [
                "Processed V1-V9 Dendrogram Baraka to Harama"
            ]
        }
    }
}
display(Image("media/Picture 10.3.png"), metadata=metadata)
```

#### Discussion: Methodological Implications
The methodology presented in this article, developed specifically for the analysis of the Hadith corpus, is broadly applicable to any textual tradition characterised by close intertextuality and significant semantic variation. This method is particularly well-suited to domains involving families of short, related texts that differ subtly in wording, emphasis, or syntactic structure—a common feature of orally transmitted religious and historical materials. In such contexts, semantic analysis moves beyond surface-level alignment to reveal underlying patterns of conceptual continuity that purely lexical collation might otherwise mask. By quantifying semantic similarity alongside structural completeness, our model enables scholars to identify not only where texts diverge orthographically but also where they diverge in conceptual content.
Despite its promise, the approach must contend with the challenges inherent in applying modern computational tools to ancient linguistic data. The underlying language model used here was not specifically fine-tuned on classical Arabic, which introduces a degree of semantic imprecision into the similarity estimates. This poses a wider challenge due to the uneven availability of specialised language models for classical languages. Although Modern Standard Arabic benefits from extensive contemporary resources, its linguistic distance from classical and Quranic Arabic remains a source of potential bias. The dominance of Modern Standard Arabic syntactic and semantic patterns can inadvertently distort the embeddings of older constructs, potentially leading to misrepresentations of meaning.

<!-- #region editable=true slideshow={"slide_type": ""} -->
## Conclusion and Future Directions
Across the corpus analysed here, the historical findings are robust: two elements consistently recur—Abraham’s sanctification of Mecca and Muhammad’s conscious emulation of this precedent in Medina. These elements, transmitted through various isnāds and preserved in diverse wordings, can plausibly be dated to the Prophet’s lifetime. This evidence supports the view that Muhammad deliberately modelled his consecration of Medina on Abraham’s perceived act, thus confirming a conscious continuation of the Abrahamic legacy. The convergence of these core elements across independent transmission lines strengthens their historical credibility; the diversity of isnāds makes large-scale fabrication implausible. Furthermore, the detection of distinct textual signatures—such as Qutayba b. Saʿīd’s interpolation of the measurement element, and Saʿīd al-Maqburī’s insertion of al-Suqyā—demonstrate the continued precision of the isnād-cum-matn analysis in tracing individual contributions within the transmission history.

The AI-based section of this study builds on, and validates, these philological foundations. By integrating Semantic Textual Similarity analysis through LLMs, the article provides a crucial tool for analysing semantic equivalence across divergent wordings, a key necessity when studying oral traditions. The strong convergence demonstrated between manual philological results and computational outcomes validates both the efficacy of traditional isnād-cum-matn analysis and the potential of LLM-assisted methods to enhance and quantify its findings.

Our model thus provides a replicable basis for studying textual transmission in any tradition characterised by close intertextuality and semantic variation. Moving forward, future research should focus on domain adaptation for Classical Arabic through the SimCSE framework, producing a specialised model optimised for Hadith and early Islamic texts. Ultimately, this study demonstrates that AI-enhanced semantic tools serve to complement, rather than replace, humanistic scholarship. When integrated with rigorous philological methods, they help us to detect significant patterns of variation and continuity across vast textual corpora, providing a deeper understanding of how early Islamic traditions were shaped, transmitted, and preserved.

<!-- #endregion -->

<!-- #region editable=true slideshow={"slide_type": ""} tags=["hidden"] -->
## Bibliography
<!-- BIBLIOGRAPHY START -->
<div class="csl-bib-body">
  <div class="csl-entry"><i id="zotero|167992/XBUBN9YB"></i>Abdelazim, H., Tharwat, M., &#38; Mohamed, A. (2023). Semantic Embeddings for Arabic Retrieval Augmented Generation (ARAG). <i>International Journal of Advanced Computer Science and Applications (IJACSA)</i>, <i>14</i>(11). <a href="https://doi.org/10.14569/IJACSA.2023.01411135">https://doi.org/10.14569/IJACSA.2023.01411135</a></div>
  <div class="csl-entry"><i id="zotero|167992/NSGSXDPG"></i>Abū al-Qāsim al-Khū’ī. (No date). <i>Mu‘jam Rijāl al-Ḥadīth wa-Tafṣīl Ṭabaqāt al-Ruwāt</i> (Vol. 11). Muassasah al-Khū’ī al-Islāmiyyah.</div>
  <div class="csl-entry"><i id="zotero|167992/VJ6FEHFL"></i>al-Baṣrī, ʿUmar b. Shabbah al-Numayrī. (1979). <i>Tārikh al-Madina</i> (F. M. Shaltūt, Ed.; Vol. 1). Dār al-Fikr.</div>
  <div class="csl-entry"><i id="zotero|167992/VETMPFAK"></i>al-Kulaynī,  al-S. (1987). <i>al-Kāfī</i> (M. Ghafārī ʿAlī Akbar Akhūndī, Ed.; Vol. 1). Dār al-Kutub al-Islāmiyya.</div>
  <div class="csl-entry"><i id="zotero|167992/6WD4V9CN"></i>al-Kulaynī, M. b. Y. b. I. (1986). <i>Al-Kāfī fī ʿIlm al-Dīn</i> (G. ʿAlī Akbar &#38; M. Ākhūndī, Eds.; Vol. 4). Dār al-Kutub al-Islāmiyya.</div>
  <div class="csl-entry"><i id="zotero|167992/3TT8KK3D"></i>al-Ṣanaʿānī, ʿAbd al-Razzāq. (1983). <i>Muṣannaf ʿAbd al-Razzāq al-Ṣanaʿānī</i> (Ḥabīb al-Raḥman al-Aʿẓamī, Ed.; Vol. 10). Maktab al-Islāmī.</div>
  <div class="csl-entry"><i id="zotero|167992/K6XYNART"></i>al-Tirmidhī, M. b. ʿĪsā b. S. (No date). <i>Sunan al-Tirmidhī</i> (Vol. 5). Dār al-Kutub al-ʿIlmīyya.</div>
  <div class="csl-entry"><i id="zotero|167992/SJB8HG5R"></i>al-Ṭūsī, M. b. al-Ḥasan. (1986). <i>Tahdhīb al-Aḥkām</i> (ʿAlī Ākhūndī &#38; M. Ākhūndī, Eds.; Vol. 6). Dār al-Kutub al-Ilmiyya.</div>
  <div class="csl-entry"><i id="zotero|167992/WDVWH94N"></i>al-Ṭūsī, M. b. al-Ḥasan. (1995). <i>Al-Amālī</i>. Dār al-Thaqāfa.</div>
  <div class="csl-entry"><i id="zotero|167992/6QISGCPU"></i>Alkaoud, M., &#38; Syed, M. (2021). Learning to Identify Narrators in Classical Arabic Texts. <i>Procedia Computer Science</i>, <i>189</i>, 335–342. <a href="https://doi.org/10.1016/j.procs.2021.05.109">https://doi.org/10.1016/j.procs.2021.05.109</a></div>
  <div class="csl-entry"><i id="zotero|167992/8HXPRMZX"></i>Andrew J. Newman. (2000). <i>The Formative Period of Twelver Shī’ism</i>. Curzon.</div>
  <div class="csl-entry"><i id="zotero|167992/P3MZ8VM9"></i>Ansari, H., &#38; Schmidtke, S. (2020). Al-Shaykh al-Ṭūsī: His Writings on Theology and their Reception. In F. Daftary &#38; G. M. Miskinzoda (Eds.), <i>Study of Shiʿi Islam: History, Theology and Law</i> (pp. 475–498). I. B. Tauris.</div>
  <div class="csl-entry"><i id="zotero|167992/2PC3DKDY"></i>Biagini, E., Geoghegan, P., Hanley, H., Jones, A., &#38; Jones, H. (2023). Reconstructing historical texts from fragmentary sources: Charles S. Parnell and the Irish crisis, 1880-86. <i>Digital Humanities Quarterly</i>, <i>017</i>(3).</div>
  <div class="csl-entry"><i id="zotero|167992/NMT2HJ8V"></i>Bordalejo, B., &#38; Vázquez, A. A. (2021). You’re Collating Just Fine and Other Lies You’ve Been Telling Yourself. <i>Digital Medievalist</i>, <i>Special Cluster 2</i>. <a href="https://doi.org/10.16995/dm.8066">https://doi.org/10.16995/dm.8066</a></div>
  <div class="csl-entry"><i id="zotero|167992/ENAMXBB7"></i>Chomsky, N. (2001). Derivation by Phase. In <i>A Life in Language</i> (pp. 1–52). MIT Press.</div>
  <div class="csl-entry"><i id="zotero|167992/J5LB4B82"></i>Comanici, G., Bieber, E., Schaekermann, M., Pasupat, I., Sachdeva, N., Dhillon, I., Blistein, M., Ram, O., Zhang, D., Rosen, E., Marris, L., Petulla, S., Gaffney, C., Aharoni, A., Lintz, N., Pais, T. C., Jacobsson, H., Szpektor, I., Jiang, N.-J., … Helmholz, W. (2025). <i>Gemini 2.5: Pushing the Frontier with Advanced Reasoning, Multimodality, Long Context, and Next Generation Agentic Capabilities</i> (arXiv:2507.06261). arXiv. <a href="https://doi.org/10.48550/arXiv.2507.06261">https://doi.org/10.48550/arXiv.2507.06261</a></div>
  <div class="csl-entry"><i id="zotero|167992/Z6674EUW"></i>de Marneffe, M.-C., Manning, C. D., Nivre, J., &#38; Zeman, D. (2021). Universal Dependencies. <i>Computational Linguistics</i>, <i>47</i>(2), 255–308. <a href="https://doi.org/10.1162/coli_a_00402">https://doi.org/10.1162/coli_a_00402</a></div>
  <div class="csl-entry"><i id="zotero|167992/VETIM69M"></i>Donner, F. M. (1998). <i>Narratives of Islamic Origins: The Beginnings of Islamic Historical Writing</i>. Darwin Press.</div>
  <div class="csl-entry"><i id="zotero|167992/RM5T3QX9"></i>Elmaz, O. (2021). An Explorative Journey Through Hadith Collections: Connecting Early Islamic Arabia with the World. <i>Journal of Arabic and Islamic Studies</i>, <i>21</i>(1), 39–56. <a href="https://doi.org/10.5617/jais.8966">https://doi.org/10.5617/jais.8966</a></div>
  <div class="csl-entry"><i id="zotero|167992/7UBWXN9I"></i>Enevoldsen, K., Chung, I., Kerboua, I., Kardos, M., Mathur, A., Stap, D., Gala, J., Siblini, W., Krzemiński, D., Winata, G. I., Sturua, S., Utpala, S., Ciancone, M., Schaeffer, M., Sequeira, G., Misra, D., Dhakal, S., Rystrøm, J., Solomatin, R., … Muennighoff, N. (2025). <i>MMTEB: Massive Multilingual Text Embedding Benchmark</i> (arXiv:2502.13595). arXiv. <a href="https://doi.org/10.48550/arXiv.2502.13595">https://doi.org/10.48550/arXiv.2502.13595</a></div>
  <div class="csl-entry"><i id="zotero|167992/TM3VTF9V"></i>Gao, T., Yao, X., &#38; Chen, D. (2022). <i>SimCSE: Simple Contrastive Learning of Sentence Embeddings</i> (arXiv:2104.08821). arXiv. <a href="https://doi.org/10.48550/arXiv.2104.08821">https://doi.org/10.48550/arXiv.2104.08821</a></div>
  <div class="csl-entry"><i id="zotero|167992/MJWFGGMR"></i>G.H.A. Juynboll. (1993). Nāfiʿ, the Mawlā of Ibn ʿUmar, and His Position in Muslim Ḥadīth Literature. <i>Der Islam</i>, <i>70</i>, 207–244.</div>
  <div class="csl-entry"><i id="zotero|167992/XVZVIRFZ"></i>Harald Motzki, Nicolet Boekhoff-van der Voort, &#38; Sean W. Anthony. (2011). <i>Analysing Muslim Traditions: Studies in Legal, Exegetical and  Maghāzī Ḥadīth</i>. Brill.</div>
  <div class="csl-entry"><i id="zotero|167992/Y58CKY2X"></i>Ḥusayn b. Muḥammad Taqī Nurī al-Ṭabarsī. (1987). <i>Mustadrak al-Wasā’il wa Mustanbaṭ al-Masā’il</i> (Vol. 4). Muassasa Āl al-Bayt.</div>
  <div class="csl-entry"><i id="zotero|167992/2QDX8ENH"></i>Ibn Anas, M. (1994). <i>Muwaṭṭaʾ</i>. Dār Iḥyāʾ al-ʿŪlūm.</div>
  <div class="csl-entry"><i id="zotero|167992/9E7TZGQQ"></i>Ibn Ḥibbān, M. (1973). <i>al-Thiqāt</i> (Vol. 5). Dāʾirat al-Maʿārif.</div>
  <div class="csl-entry"><i id="zotero|167992/7859R5XF"></i>Ibn Māja, M. b. Y. (1954). <i>Sunan Ibn Mājah</i> (Vol. 2). al-Maktab al-ʿIlmiyya.</div>
  <div class="csl-entry"><i id="zotero|22122298/7ANRDQPP"></i>Jaro, M. A. (1989). Advances in Record-Linkage Methodology as Applied to Matching the 1985 Census of Tampa, Florida. <i>Journal of the American Statistical Association</i>, <i>84</i>(406), 414–420. <a href="https://doi.org/10.1080/01621459.1989.10478785">https://doi.org/10.1080/01621459.1989.10478785</a></div>
  <div class="csl-entry"><i id="zotero|167992/3FBRBUHQ"></i>Kister, M. J. (2018). Musaylima. In <i>Encyclopaedia of the Qur’ān Online</i>. Brill. <a href="https://doi.org/10.1163/1875-3922_q3_EQSIM_00293">https://doi.org/10.1163/1875-3922_q3_EQSIM_00293</a></div>
  <div class="csl-entry"><i id="zotero|167992/8UA474YW"></i>Kuzudişli, B. (2015). Sunnī-Shīʿī Interaction in the Early Period “ The Transition of the Chains of Ahl al-Sunna to the Shīʿa “. <i>Ilahiyat Studies</i>, <i>6</i>(1), 7–45. <a href="https://www.ilahiyatstudies.org/index.php/journal/article/view/264">https://www.ilahiyatstudies.org/index.php/journal/article/view/264</a></div>
  <div class="csl-entry"><i id="zotero|167992/6NDAKMYI"></i>Māzandarānī, M. H. B. (2009). ʿIddatun al-Kulayni wa-Isnāduhu. In M. Ganbarī (Ed.), <i>Shenakht-nameh-e Kulaynī va al-Kāfī</i> (Vol. 3, pp. 439–500). Sāzmān-i Chap va Nashr.</div>
  <div class="csl-entry"><i id="zotero|167992/VN9EYQ7L"></i>Mikolov, T., Chen, K., Corrado, G., &#38; Dean, J. (2013). <i>Efficient Estimation of Word Representations in Vector Space</i> (arXiv:1301.3781). arXiv. <a href="https://doi.org/10.48550/arXiv.1301.3781">https://doi.org/10.48550/arXiv.1301.3781</a></div>
  <div class="csl-entry"><i id="zotero|167992/RFDAPQNS"></i>Modarressi, H. (2022). <i>Text and Interpretation: Imam Jaʿfar al-Ṣādiq and His Legacy in Islamic Law</i>. Harvard University Press.</div>
  <div class="csl-entry"><i id="zotero|167992/4XF2D7I4"></i>Munt, H. (2014). <i>The Holy City of Medina: Sacred Space in Early Islamic Arabia</i>. Cambridge University Press.</div>
  <div class="csl-entry"><i id="zotero|167992/DUKWBJJM"></i>Muslim b. al-Ḥajjāj al-Qushayrī. (1991). <i>Ṣaḥīh Muslim</i> (Vol. 4). Dār Iḥyā’.</div>
  <div class="csl-entry"><i id="zotero|167992/DWF9N3UN"></i>Nacar, O., &#38; Koubaa, A. (2024). <i>Enhancing Semantic Similarity Understanding in Arabic NLP with Nested Embedding Learning</i> (arXiv:2407.21139). arXiv. <a href="https://doi.org/10.48550/arXiv.2407.21139">https://doi.org/10.48550/arXiv.2407.21139</a></div>
  <div class="csl-entry"><i id="zotero|167992/99LZUX7J"></i>Nie, Z., Feng, Z., Li, M., Zhang, C., Zhang, Y., Long, D., &#38; Zhang, R. (2025). <i>When Text Embedding Meets Large Language Model: A Comprehensive Survey</i> (arXiv:2412.09165). arXiv. <a href="https://doi.org/10.48550/arXiv.2412.09165">https://doi.org/10.48550/arXiv.2412.09165</a></div>
  <div class="csl-entry"><i id="zotero|167992/J24STZ3B"></i>Niyogi, S. (2001). A Minimalist Implementation of Verb Subcategorization. In <i>Proceedings of the Seventh International Workshop on Parsing Technologies</i> (pp. 142–153). International Workshop on Parsing Technologies.</div>
  <div class="csl-entry"><i id="zotero|167992/DLCIQE2K"></i>Pagolu, A. (1998). <i>The Religion of the Patriarchs</i>. A&#38;C Black.</div>
  <div class="csl-entry"><i id="zotero|167992/7GWKJ6GM"></i>Pennington, J., Socher, R., &#38; Manning, C. (2014). GloVe: Global Vectors for Word Representation. In A. Moschitti, B. Pang, &#38; W. Daelemans (Eds.), <i>Proceedings of the 2014 Conference on Empirical Methods in Natural Language Processing (EMNLP)</i> (pp. 1532–1543). Association for Computational Linguistics. <a href="https://doi.org/10.3115/v1/D14-1162">https://doi.org/10.3115/v1/D14-1162</a></div>
  <div class="csl-entry"><i id="zotero|167992/HUMCFE4N"></i>Reimers, N., &#38; Gurevych, I. (2019). <i>Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks</i> (arXiv:1908.10084). arXiv. <a href="https://doi.org/10.48550/arXiv.1908.10084">https://doi.org/10.48550/arXiv.1908.10084</a></div>
  <div class="csl-entry"><i id="zotero|167992/SMDM2A7L"></i>Shamsy, A. E. (2021). The Ur-Muwaṭṭaʾ and Its Recensions. <i>Islamic Law and Society</i>, <i>28</i>(4), 352–381. <a href="https://doi.org/10.1163/15685195-bja10011">https://doi.org/10.1163/15685195-bja10011</a></div>
  <div class="csl-entry"><i id="zotero|167992/S4PK974Q"></i>Tafrīshī, M. T. (1990). <i>Naqd al-rijāl</i> (Vol. 7). Jāmiʿa Mudarrisīn.</div>
  <div class="csl-entry"><i id="zotero|167992/42F7HEVV"></i>Thakur, N., Reimers, N., Daxenberger, J., &#38; Gurevych, I. (2021). <i>Augmented SBERT: Data Augmentation Method for Improving Bi-Encoders for Pairwise Sentence Scoring Tasks</i> (arXiv:2010.08240). arXiv. <a href="https://doi.org/10.48550/arXiv.2010.08240">https://doi.org/10.48550/arXiv.2010.08240</a></div>
  <div class="csl-entry"><i id="zotero|167992/ZRNIKFUI"></i>Vylomova, E., Rimell, L., Cohn, T., &#38; Baldwin, T. (2016). <i>Take and Took, Gaggle and Goose, Book and Read: Evaluating the Utility of Vector Differences for Lexical Relation Learning</i> (arXiv:1509.01692). arXiv. <a href="https://doi.org/10.48550/arXiv.1509.01692">https://doi.org/10.48550/arXiv.1509.01692</a></div>
  <div class="csl-entry"><i id="zotero|167992/7LTBFGEP"></i>Wang, L., Yang, N., Huang, X., Jiao, B., Yang, L., Jiang, D., Majumder, R., &#38; Wei, F. (2024). <i>Text Embeddings by Weakly-Supervised Contrastive Pre-training</i> (arXiv:2212.03533). arXiv. <a href="https://doi.org/10.48550/arXiv.2212.03533">https://doi.org/10.48550/arXiv.2212.03533</a></div>
  <div class="csl-entry"><i id="zotero|167992/WQNAPATM"></i>Wang, L., Yang, N., Huang, X., Yang, L., Majumder, R., &#38; Wei, F. (2024). <i>Multilingual E5 Text Embeddings: A Technical Report</i> (arXiv:2402.05672). arXiv. <a href="https://doi.org/10.48550/arXiv.2402.05672">https://doi.org/10.48550/arXiv.2402.05672</a></div>
  <div class="csl-entry"><i id="zotero|167992/68L2J228"></i>Webb, P. A. (2024). The History and Significance of the Meccan Hajj: from Pre-Islam to the Rise of the Abbasids. In Q. M. Khan &#38; N. Nassar (Eds.), <i>Hajj and the Arts of Pilgrimage : Essays in Honour of Nasser David Khalili</i> (pp. 28–46). Gingko. <a href="https://hdl.handle.net/1887/3663458">https://hdl.handle.net/1887/3663458</a></div>
  <div class="csl-entry"><i id="zotero|22122298/TG9N5K8T"></i>Winkler, W. (1990). <i>(PDF) String Comparator Metrics and Enhanced Decision Rules in the Fellegi-Sunter Model of Record Linkage</i>. ResearchGate. <a href="https://www.researchgate.net/publication/243772975_String_Comparator_Metrics_and_Enhanced_Decision_Rules_in_the_Fellegi-Sunter_Model_of_Record_Linkage">https://www.researchgate.net/publication/243772975_String_Comparator_Metrics_and_Enhanced_Decision_Rules_in_the_Fellegi-Sunter_Model_of_Record_Linkage</a></div>
  <div class="csl-entry"><i id="zotero|167992/IHEIIWZR"></i>Zhang, Y., Li, M., Long, D., Zhang, X., Lin, H., Yang, B., Xie, P., Yang, A., Liu, D., Lin, J., Huang, F., &#38; Zhou, J. (2025). <i>Qwen3 Embedding: Advancing Text Embedding and Reranking Through Foundation Models</i> (arXiv:2506.05176). arXiv. <a href="https://doi.org/10.48550/arXiv.2506.05176">https://doi.org/10.48550/arXiv.2506.05176</a></div>
</div>
<!-- BIBLIOGRAPHY END -->
<!-- #endregion -->
