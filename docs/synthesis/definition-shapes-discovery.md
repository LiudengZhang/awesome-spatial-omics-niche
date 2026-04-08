# Definition Shapes Discovery

## The Core Thesis

Every spatial niche method makes an implicit choice about what a niche is. That choice determines what the method can discover.

This is not a flaw — it is the nature of any measurement. A microscope reveals what its resolution can see. A niche method reveals what its definition can capture.

The problem is that most papers do not make this choice explicit. A method paper says "we identify cellular niches" without specifying which of several possible niche definitions it uses. A benchmark paper compares methods without acknowledging that they define the target concept differently. A review paper lists methods as if they are solving the same problem when they are answering different questions.

## The Evidence

Consider three methods applied to the same colorectal cancer tissue:

**CellCharter** (composition-based) finds 8 neighborhoods defined by cell-type proportions. The "tumor-immune interface" neighborhood contains 40% tumor cells, 25% CD8 T cells, 20% macrophages, 15% other. Every location with this composition gets the same label.

**BANKSY** (expression-based) subdivides that interface into 3 sub-regions based on expression gradients. The tumor side has proliferative markers. The immune side has activation markers. The boundary itself has a unique gradient signature. These are invisible to composition methods.

**NicheCompass** (communication-based) reveals that only a fraction of the interface has active PD-L1/PD-1 signaling. The rest has active CXCL9/CXCR3 signaling. Communication-based, these are two completely different niches. Composition-based, they are the same.

Which is "correct"? All of them. They are answering different questions:

- CellCharter answers: **Where do these cell types co-localize?**
- BANKSY answers: **Where does expression change?**
- NicheCompass answers: **Where are cells communicating?**

## Implications for Research

### For Method Developers

State your niche definition explicitly. Not "we identify niches" but "we define niche as [specific definition] and identify instances of this definition." This makes the method's scope clear and its limitations honest.

### For Benchmarkers

Do not compare methods with different niche definitions on the same ground truth. A composition-based and a communication-based method are not solving the same problem. Fair comparison requires either:

1. Methods with the same niche definition compared on appropriate ground truth, or
2. Cross-definition comparison with biological validation (do the different niches predict the same biological outcomes?).

### For Users

Match your niche definition to your biological question. If you care about cell-type neighborhoods, use composition methods. If you care about signaling, use communication methods. If you are exploring, try multiple definitions and look for convergent structure — niches identified by multiple definitions are likely biologically robust.

## The Meta-Insight

The diversity of niche definitions is not a problem to be solved — it is a reflection of the genuine complexity of tissue microenvironments. A niche is not a single thing. It is a cell-type neighborhood AND an expression microenvironment AND a signaling context AND a morphological region. Different methods illuminate different facets of this multidimensional reality.

The field's maturation will come not from converging on a single definition, but from understanding which definition is right for which question — and from methods that integrate multiple definitions into a unified framework (as NicheCompass begins to do).
