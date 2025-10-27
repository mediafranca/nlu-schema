# PictoNet NLU Schema · v1.0

**PictoNet NLU Schema** defines a structured contract between the *Natural Language Understanding (NLU) front-end* and the *SVG compiler* within the [PictoNet](https://pictos.net) ecosystem.  
It encodes communicative intent, semantic roles, logical form, and visual grounding cues — allowing human utterances to be transformed into cognitively accessible pictograms.

## Purpose

This schema formalises how linguistic meaning is represented before being rendered pictographically.  
It bridges multiple traditions in linguistic semantics:

| Layer | Theoretical basis | Schema component |
|-------|-------------------|------------------|
| Speech Act & Intent | Austin · Searle · ISO 24617-2 | `metadata.speech_act`, `metadata.intent` |
| Frame Semantics | Fillmore · FrameNet | `frames[*].roles` |
| Logical Representation | AMR / MRS | `logical_form` |
| Semantic Primes | Wierzbicka · Goddard (NSM) | `nsm_explictations` |
| Pragmatics | Brown & Levinson, ISO 24617-2 | `pragmatics` |
| Visual Grounding | Scene Graphs · AAC pictography | `visual_guidelines` |

Each utterance analysed through PictoNet’s NLU front-end is serialised into a single JSON object conforming to this schema.  
That object becomes the semantic input for the pictogram compiler, ensuring transparent, reproducible mapping between text, meaning, and image.

## Example

```json
{
  "utterance": "I want you to make the bed",
  "lang": "en",
  "metadata": { "speech_act": "directive", "intent": "request" },
  "frames": [
    {
      "id": "f1",
      "frame_name": "Directed_action",
      "lexical_unit": "make",
      "roles": {
        "Agent": { "type": "Addressee", "ref": "you", "surface": "you" },
        "Theme": { "type": "Object", "lemma": "bed", "surface": "the bed" }
      }
    },
    {
      "id": "f2",
      "frame_name": "Desire",
      "lexical_unit": "want",
      "roles": {
        "Experiencer": { "type": "Speaker", "ref": "I", "surface": "I" },
        "DesiredEvent": { "type": "Event", "ref_frame": "f1" }
      }
    }
  ],
  "nsm_explictations": {
    "WANT": "I feel something. I don’t have something. I want it to happen.",
    "DO": "Someone does something.",
    "BED": "Something used for sleeping."
  },
  "logical_form": { "event": "make(you, bed)", "modality": "want(I, event)" },
  "pragmatics": { "politeness": "neutral", "formality": "informal", "expected_response": "compliance" },
  "visual_guidelines": { "focus_actor": "you", "context": "bedroom", "temporal": "immediate" }
}
```

## Schema Structure
 - utterance – the original text
 - lang – language tag (BCP-47)
 - metadata – speech act, intent, optional timestamp and speaker ID
 - frames – array of FrameNet-style frame objects with roles
 - nsm_explictations – Natural Semantic Metalanguage decomposition
 - logical_form – formal predicate representation
 - pragmatics – tone, politeness, formality, expected response
 - visual_guidelines – cues for layout and pictogram design

A complete formal definition is provided in [pictonet-nlu-1.0.schema.json](pictonet-nlu-1.0.schema.json).

## Licence

Released under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE) licence.
You are free to copy, modify, and redistribute this schema with attribution to
Herbert Spencer González · mediafranca.org
