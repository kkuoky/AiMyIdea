
[🇰🇷 한국어](https://www.google.com/search?q=%23-korean) | [🇬🇧 English](https://www.google.com/search?q=%23-english)

<a name="-korean"></a>
# "Elements of AI - part2 Building AI course" 최종 프로젝트

**[buildingai.elementsofai.com](https://buildingai.elementsofai.com/)**

## 요약

본 프로젝트는 AI가 생성한 이미지와 인간이 만든 이미지를 구별하도록 훈련된 **AI 분류 모델**입니다. 이 모델은 업로드된 이미지를 분석하여, 해당 이미지가 AI에 의해 합성되었는지(AI-generated) 아니면 사람이 직접 만들었는지(Human-made) 판별합니다.

## 배경

최근 Midjourney, Stable Diffusion 등 생성형 AI 모델의 발전으로 인해, AI가 만든 이미지와 실제 사진 또는 인간 아티스트의 디지털 창작물을 구분하기가 점점 더 어려워지고 있습니다.

이러한 현상은 다음과 같은 문제가 있습니다.

- 가짜 뉴스: 실제 사건처럼 조작된 AI 이미지로 인해 사실이 왜곡되고 사람들이 선동될 수 있습니다.
    
- 창작자의 지식재산권 침해: 첫째, 기존 저작권자의 동의를 받지 않은 데이터를 학습에 활용한 AI 프로젝트들은 저작권법을 위반한 것입니다. 둘째, AI가 생성한 이미지를 인간 창작물로 사칭하는 행위는 새로운 창작자들의 창의력과 생존에 영향을 줄 수 있습니다. 
    
- 사회적 신뢰도 하락: 이미, 유튜브 등 사용자 생성형 멀티미디어 서비스에는 AI로 생성한 콘텐츠가 범람하고 있어서, 무엇이 진짜이고 무엇이 가짜인지 구별할 수 없게 되면서 온라인상의 콘텐츠 신뢰도하락과 노이즈의 증가로 사회적 신뢰 회복 비용이 증가할 수 있습니다.
    
    본 프로젝트는 이러한 문제의 심각성을 인지하고, 디지털 콘텐츠의 투명성과 진위 여부를 가릴 수 있는 기술적 도구가 필요하다는 목적에서 시작되었습니다.
    

## 활용 예시

이 솔루션은 온프레미스 형태로 활용되는 것을 목적으로 오픈소스 형태로 공개될 것입니다.

1. 사용자(예: 서비스 프로바이더, 뉴스 편집자, 멀티미디어 편집자, 일반 사용자)가 진위 여부를 확인하고 싶은 이미지를 업로드하거나 이미지의 웹링크를 제공합니다.
    
    .   
    
2. 사전학습된 AI 모델이 이미지를 분석하여 AI가 생성한 이미지인지를 확률적으로 판단합니다.
    
3. AI 모델이 "AI 생성 확률: 51%" 또는 "인간 제작 확률: 95%"와 같이 확률로 결과값을 반환합니다.
    

이 툴은 언론사가 보도 사진을 검증하거나, 아트 공모전에서 AI 작품을 스크리닝하거나, 소셜 미디어 플랫폼이 딥페이크(Deepfake) 콘텐츠를 필터링해야 하는 상황에서 유용하게 사용될 수 있습니다.

## 훈련용 데이터와 인공신경망의 구조

본 모델은 **지도 학습(Supervised Learning)** 기반의 **CNN(합성곱 신경망)** 아키텍쳐로 설계할 것입니다. CNN은 이미지의 공간적 특징을 효율적으로 학습할 수 있어 이미지 분류 작업에 매우 적합합니다.

학습 데이터는 무료 이미지 사이트에서 수집한 데이터와 Stable Deffusion 등 AI로 생성한 데이터를 병행하여 학습을 시킨 후, 사용자의 서비스 이용과정에서 수집되는 이미지들 중에 소정의 확률에 포함되는 이미지들을 추가 수집하여 AI 모델을 업그레이드 하고, 새로운 가중치를 계속 공개할 것입니다.  

1. Human-Made Images: 실제 사진 데이터셋(예: COCO, Unsplash)과 인간 아티스트들의 디지털 아트워크 및 서비스 이용자가 검증 요청한 이미지 중에 AI 생성 확률이 20% 이하인 이미지
    
2. AI-Generated Images: 다양한 최신 생성 모델(Stable Diffusion, Midjourney, DALL-E 3 등)을 통해 생성된 대규모 이미지셋 및 및 서비스 이용자가 검증 요청한 이미지 중에 AI 생성 확률이 80% 이상인 이미지
    
    | AI 방법             | 설명                                                 |
    
    | --------------------- | ------------------------------------------------------ |
    
    | CNN Classifier    | 이미지의 미세한 패턴과 통계적 차이를 학습하여 두 클래스(AI vs Human)를 분류하는 신경망 |
    
    | Data Augmentation | 데이터셋의 다양성을 확보하기 위해 이미지를 회전, 확대/축소, 압축하는 등의 '데이터 증강' 기법 |
    

## 도전 과제

이 프로젝트는 다음과 같은 현실적 한계와 윤리적 문제가 있어서 현재로서는 완벽하지 않습니다.

- 창과 방패의 싸움: 이 검출기술이 AI 생성 패턴을 학습하더라도, 미래의 AI 생성 모델은 이 탐지를 피하도록 더 정교하게 진화할 것입니다.(GAN 모델의 생성자와 판별자 처럼) 따라서 탐지 모델은 지속적인 업데이트가 필요하고, 데이터의 품질(명확하게 인간의 창작 작품과 AI가 생성한 이미지)이 중요하고 이를 확보하는 객관적인 방법이 필요합니다.
    
- 후처리(Post-processing)에 대한 취약성: AI가 생성한 이미지를 사람이 리터칭 하거나 이미지 툴로 필터를 적용 등 인간의 후보작업물에 대한 판단이 미숙할 수 있습니다.
    
- 윤리적 문제 (False Positives): 가장 큰 위험은 **'잘못된 AI 판정'**입니다. 이 모델이 인간 창작물을 "AI 생성"이라고 잘못 분류할 경우, 해당 아티스트의 명예와 지식재산권에 심각한 피해를 줄 수 있습니다. 따라서 이 툴은 '최종 판결'이 아닌 '보조적인 참고 자료'로만 사용되어야 하고, 종국에 모든 중요한 결정은 인간이 하여야 합니다.
    

## 향후 과제

이 프로젝트는 향후 다음과 같이 발전할 수 있습니다.

- 탐지 콘텐츠 영역 확장: 이미지뿐만 아니라 AI 생성 비디오(딥페이크) 및 AI 생성 음성까지 탐지할 수 있도록 모델을 확장할 수 있습니다만 매우 어려운 과제가 될 것입니다.
    
- 탐지 데이터 영역 확장: 생성된 이미지뿐만 아니라 이미지 생성 AI 모델 학습에 사용된 학습용 이미지 데이터까지 추론할 수 있다면 창작자의 저작권 보호에 더욱 많은 공헌을 할 수 있습니다.
    

## 감사의 말씀

- 본 프로젝트는 핀란드 헬싱키 대학(University of Helsinki)과 Reaktor Innovations가 제공하는 Elements of AI - Building AI 무료 온라인 과정의 최종 프로젝트로 제출되었습니다.
    
- 본 아이디어는 [YuE](https://github.com/multimodal-art-projection/YuE), [stable-diffusion](https://github.com/CompVis/stable-diffusion), [vits](https://github.com/jaywalnut310/vits), **[Bert-VITS2](https://github.com/fishaudio/Bert-VITS2)** 등 음성, 이미지, 음악 등 멀티미디어를 생성하는 AI 프로젝트를 리뷰하는 과정에서 영감을 받았습니다.
    
- 철학, 감정, 기억, 감각에 기반한 인간의 창작물과 복잡한 계산 과정을 거친 확률값에 기반하여 생성하는 AI의 결과값은 구분되어야 한다고 생각합니다. 그런 의미에서 나에게 많은 감동을 준 수많은 작가, PD, 만화가, 영화감독, 배우 등에게 고맙습니다. 나는 당신의 지식재산권 보호를 위해 노력하겠습니다.
    
- 본 아이디어는 YuE, stable-diffusion, vits, Bert-VITS2 등 음성, 이미지, 음악 등 멀티미디어를 생성하는 AI 프로젝트를 리뷰하는 과정에서 영감을 받았습니다. 관련 프로젝트에 기여한 모든 엔지니어들에게 감사의 말씀을 전합니다.
    

---

---

<a name="-english"></a>

# "Elements of AI - part2 Building AI course" Final Project

**[buildingai.elementsofai.com](https://buildingai.elementsofai.com/)**

## Summary

This project is an **AI classification model** trained to distinguish between images generated by AI and those created by humans. This model analyzes an uploaded image to determine if it was synthesized by AI (AI-generated) or created by a person (Human-made).

## Background

Recently, with the advancement of generative AI models like Midjourney and Stable Diffusion, it has become increasingly difficult to distinguish between AI-made images and actual photographs or digital creations by human artists.

This phenomenon presents the following problems:

- **Fake News:** Fabricated AI images, appearing as real events, can distort facts and be used to incite people.
    
- **Infringement on Creators' Intellectual Property:** First, AI projects that use data for training without the consent of the original copyright holders are in violation of copyright law. Second, the act of passing off AI-generated images as human creations can impact the creativity and livelihood of new creators.
    
- **Decline in Social Trust:** Already, user-generated multimedia services like YouTube are flooded with AI-generated content. The inability to distinguish truth from falsehood can lead to a decline in the trustworthiness of online content and an increase in noise, raising the social cost of restoring trust.
    

This project was started with the objective of addressing the severity of these issues and the need for a technical tool to ensure the transparency and authenticity of digital content.

## How is it used?

This solution will be released as an open-source project intended for on-premise use.

1. A user (e.g., service provider, news editor, multimedia editor, or general user) uploads an image or provides a web link for the image they wish to authenticate.
    
2. The pre-trained AI model analyzes the image to probabilistically determine if it is AI-generated.
    
3. The AI model returns a probabilistic result, such as "AI Generation Probability: 51%" or "Human-Made Probability: 95%."
    

This tool can be usefully employed in situations where news organizations need to verify press photos, art competitions need to screen for AI-generated works, or social media platforms need to filter Deepfake content.

## Data sources and AI methods

This model will be designed with a **CNN (Convolutional Neural Network)** architecture based on **Supervised Learning**. CNNs are highly suitable for image classification tasks as they can efficiently learn the spatial features of an image.

The training data will be collected from free image sites and generated by AI (like Stable Diffusion) for initial training. Subsequently, the model will be upgraded by collecting a small percentage of images submitted by users during the service's operation, and new model weights will be continuously released.

1. **Human-Made Images:** Datasets of real photographs (e.g., COCO, Unsplash) and digital artwork by human artists, as well as images submitted by users that have an AI generation probability of 20% or less.
    
2. **AI-Generated Images:** Large-scale image sets generated by various modern generative models (Stable Diffusion, Midjourney, DALL-E 3, etc.) and images submitted by users with an AI generation probability of 80% or more.
    

|**AI Method**|**Description**|
|---|---|
|**CNN Classifier**|A neural network that learns the subtle patterns and statistical differences in images to classify them into two classes (AI vs. Human).|
|**Data Augmentation**|Techniques such as rotating, scaling, and compressing images are used to ensure dataset diversity.|

## Challenges

This project is not perfect as it faces the following practical limitations and ethical issues:

- **An "Arms Race" (Cat-and-Mouse Game):** Even if this detection technology learns AI-generated patterns, future AI generation models will evolve to evade this detection (much like the Generator and Discriminator in a GAN model). Therefore, the detection model requires continuous updates, and the quality of data (clearly distinct human vs. AI images) and an objective method to secure it are crucial.
    
- **Vulnerability to Post-processing:** The model's judgment may be naive when faced with human post-processing, such as an AI-generated image being retouched by a person or having filters applied in an image tool.
    
- **Ethical Problem (False Positives):** The greatest risk is a 'false positive' AI determination. If this model incorrectly classifies a human creator's work as "AI-generated," it can cause severe damage to that artist's reputation and intellectual property. Therefore, this tool must only be used as a supplementary reference, not as a final verdict. All critical decisions must ultimately be made by a human.
    

## What next?

This project can be expanded in the following ways in the future:

- **Expanding Detection Content:** The model could be expanded to detect not only images but also **AI-generated videos (Deepfakes)** and **AI-generated audio**, though this will be a very difficult task.
    
- **Expanding Detection Data:** If the model could infer not just the generated image but also the training data used by the image generation AI, it could contribute even more to protecting creator copyrights.
    

## Acknowledgments

- This project is submitted as the final project for the [Elements of AI - Building AI](https://www.google.com/search?q=https://www.elementsofai.com/build-ai) free online course, provided by the University of Helsinki and Reaktor Innovations.
    
- This idea was inspired while reviewing multimedia-generating AI projects such as [YuE](https://github.com/multimodal-art-projection/YuE), [stable-diffusion](https://github.com/CompVis/stable-diffusion), [vits](https://github.com/jaywalnut310/vits), and **[Bert-VITS2](https://github.com/fishaudio/Bert-VITS2)**.
    
- I believe that a distinction must be made between human creations based on philosophy, emotion, memory, and senses, and the results of AI, which are generated based on probabilistic values from complex calculations. In that sense, I am grateful to the countless authors, producers, cartoonists, film directors, and actors who have moved me. I will strive to protect your intellectual property.
    
- I extend my gratitude to all the engineers who contributed to the related projects that inspired this idea, such as [YuE](https://github.com/multimodal-art-projection/YuE), [stable-diffusion](https://github.com/CompVis/stable-diffusion), [vits](https://github.com/jaywalnut310/vits), and **[Bert-VITS2](https://github.com/fishaudio/Bert-VITS2)**.
