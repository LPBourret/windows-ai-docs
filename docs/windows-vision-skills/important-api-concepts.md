---
author: eliotcowley
title: Important API concepts
description: Learn about important concepts for the Windows Vision Skills API.
ms.author: elcowle
ms.date: 4/25/2019
ms.topic: article
keywords: windows 10, windows ai, windows vision skills
ms.localizationpriority: medium
---

# Important API concepts

> [!NOTE]
> Some information relates to pre-released product, which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.

The [Microsoft.AI.Skills.SkillInterfacePreview](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview) namespace provides a set of base interfaces to be extended by all skills as well as classes and helper methods for skill implementers on Windows.

This API is meant to streamline the way skills work and how developers interact with them. The goal is to abstract away the API specificities of each skill's logic and leverage instead a consistent developer intuition over a broad set of solutions and applications. It is also meant to leverage Windows primitives which eases interop with OS APIs. This constitutes a template to derive from that standardizes the developer flow and API interactions across all skills.

Therefore all skills are expected to implement at the very least the following interfaces:

<br/>

| Name | Description |
|------|-------------|
| [ISkillDescriptor](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskilldescriptor) | Provides information on the skill and exposes its requirements (input, output, version, etc). Acts as a factory for the [ISkill](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskill). |
| [ISkillBinding](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillbinding) | Serves as an indexed container of [ISkillFeature](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeature). It acts as a conduit for input and output variables to be passed back and forth to the [ISkill](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskill). It handles pre-processing and post-processing of the input/output data and simplifies their access. |
| [ISkill](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskill) | Exposes the core logic of processing its input and forming its output via an [ISkillBinding](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillbinding). Acts as a factory for the [ISkillBinding](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillbinding). |

Skills are meant to optimally leverage the hardware capabilities (CPU, GPU, etc.) of each system they run on. These hardware acceleration devices are represented by deriving the [ISkillExecutionDevice](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillexecutiondevice) interface associated with a [SkillExecutionDeviceKind](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.skillexecutiondevicekind). Therefore, [ISkillDescriptor](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskilldescriptor) derivatives have to find, filter, and expose the set of [ISkillExecutionDevice](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillexecutiondevice) objects currently available on the host system that can execute the [ISkill](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskill) logic.

This will allow the developer consuming the skill package to best chose how to tap into supported hardware resources at runtime on a user's system. There are some [ISkillExecutionDevice](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillexecutiondevice) derivative classes already defined and available for convenience ([SkillExecutionDeviceCPU](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.skillexecutiondevicecpu) and [SkillExecutionDeviceDirectX](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.skillexecutiondevicedirectx)).

In order to ensure that input and output variables are passed in the correct format to the skill, the [ISkillFeature](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeature) interface is defined. It is meant to encapsulate a value in a predefined format. This value is represented by a derivative of [ISkillFeatureValue](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeaturevalue), which has multiple common derivatives already defined and available for convenience ([ISkillFeatureTensorValue](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeaturetensorvalue), [SkillFeatureImageValue](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.skillfeatureimagevalue), and [SkillFeatureMapValue](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.skillfeaturemapvalue)).

Each [ISkillFeatureValue](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeaturevalue) derivative has a corresponding [ISkillFeatureDescriptor](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeaturedescriptor) derivative defined that describes the expected format supported by the skill ([ISkillFeatureTensorDescriptor](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeaturetensordescriptor), [ISkillFeatureImageDescriptor](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeatureimagedescriptor), and [ISkillFeatureMapDescriptor](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeaturemapdescriptor)). These [ISkillFeatureDescriptor](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeaturedescriptor) derivatives also act as factory objects for the [ISkillFeatureValue](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeaturevalue) objects they describe.

At instantiation time, if the primitive used to assign to [ISkillFeatureValue](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeaturevalue) does not match the description provided by its [ISkillFeatureDescriptor](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeaturedescriptor) counterpart, an automatic conversion specific to each type can occur seamlessly (for example, transcoding when binding an image in a different format than the one required, or cropping if the aspect ratio differs).

## API flow <a name="APIFlow"></a>

Since all skills derive the same set of base interfaces from [Microsoft.AI.Skills.SkillInterfacePreview](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview), they all follow the same flow which usually boils down to the following operations:

<div style="text-align:center" markdown="1">

![Windows Vision Skill API flow diagram showcasing the usual sequence of API calls from an application ingesting a Windows Vision Skill](../images/vision-skills-flow.png)

</div>

1) Instantiate the [ISkillDescriptor](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskilldescriptor) derivative.

2) Query the available execution devices using the instance from step 1 (using [ISkillDescriptor.GetSupportedExecutionDevicesAsync](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskilldescriptor.getsupportedexecutiondevicesasync)) or skip and directly attempt step 3.

3) Instantiate the skill using the instance from step 1 and the desired execution device from step 2 (using [ISkillDescriptor.CreateSkillAsync(ISkillExecutionDevice)](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskilldescriptor.createskillasync)) or the default one decided by the skill developer (using [ISkillDescriptor.CreateAsync()](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskilldescriptor.createskillasync)).

4) Instantiate a skill binding object using the skill instance from step 3 (using [ISkill.CreateSkillBindingAsync](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskill.createskillbindingasync)).

5) Retrieve your primitives ([VideoFrame](https://docs.microsoft.com/uwp/api/windows.media.videoframe), [IVectorView](https://docs.microsoft.com/uwp/api/windows.foundation.collections.ivectorview_t_), [IMapView](https://docs.microsoft.com/uwp/api/windows.foundation.collections.imapview_k_v_), etc.) from your application and bind them to your binding object from step 4 (by accessing the corresponding [ISkillFeature](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeature) indexed via its name and calling [ISkillFeature.SetFeatureValueAsync(Object)](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeature.setfeaturevalueasync)).

6) Run your skill over your binding object (by calling [ISkill.EvaluateAsync](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskill.evaluateasync)).

7) Retrieve your output primitives from your binding object instantiated in step 4 (by accessing the corresponding [ISkillFeature](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeature) indexed via its name and calling the getter [ISkillFeature.FeatureValue](https://docs.microsoft.com/dotnet/api/microsoft.ai.skills.skillinterfacepreview.iskillfeature.featurevalue)).

8) Rinse and repeat from step 5 using the same binding object and new primitives.

To see this in action, refer to the tutorial on ingesting a [Windows Vision Skill from an app](tutorial1.md).

[!INCLUDE [help](../includes/get-help-vision.md)]
