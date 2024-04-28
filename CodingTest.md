## 문제 1

동이는 졸업을 위해 논문을 작성하고 있습니다. 논문은 학회마다 정해진 양식이 다르고, 동이는 이 사실을 몰랐기 때문에 논문을 전체적으로 많이 수저해야 합니다.

수정해야 하는 사항 중 하나는 바로 참조 문헌 양식입니다.
동이는 아래와 같이 각 문장의 참조 문헌을 모두 제목으로 나열하여 논문을 작성하였습니다.
<br />
**수정 전**

> Deeper neural networks are more difficult to train. We present a residual learning framework to ease the training of networks that are substantially deeper than those used previously.**[ some_paper_a, some_paper_b ]** We explicitly reformulate the layers as learning residual functions with reference to the layer inputs, instead of learning unreferenced functions.**[ some_book_a, some_paper_a ]** We provide comprehensive empirical evidence showing that these residual networks are easier to optimize, and can gain accuracy from considerably increased depth. **[ some_book_b ]**

<br />

하지만 동이가 제출하려는 학회에서는 참조한 모든 문헌을 논문의 끝에 참조된 순서대로 번호를 부여하여 나열해야 합니다. 그리고 참조가 된 부분에서 문헌의 제목이 아닌 문헌의 번호들을 오름차순으로 쉼표로 구분해서 나열해야 합니다. 논문의 번호는 원본 논문에서 처음으로 언급 된 순서대로 부여합니다.

<br />

**수정 후**

> Deeper neural networks are more difficult to train. We present a residual learning framework to ease the training of networks that are substantially deeper than those used previously.**[ 1, 2 ]** We explicitly reformulate the layers as learning residual functions with reference to the layer inputs, instead of learning unreferenced functions.**[ 1, 3 ]** We provide comprehensive empirical evidence showing that these residual networks are easier to optimize, and can gain accuracy from considerably increased depth. **[ 4 ]** 

<br />

밤샘 연구로 지친 동이는 논문을 수정할 기운이 없습니다. 불쌍한 대학원생 동이를 위해 자동으로 양식을 수정해주는 프로그램을 작성해주세요.

---

#### 입력 형식

모든 입력은 표준 입력을 통해 주어지며 한 문단의 논문이 입력으로 주어집니다. 문단의 길이는 최대 2000글자를 넘지 않고, 들여쓰기나, 앞뒤의 공백이 존재하지 않습니다.

<br />

논문의 본문 내용은 영어로 작성되어 있으며, 참조 문헌 언급 이외에 다른 용도의 괄호는 존재하지 않습니다. 참조 분헌의 목록은 항상 두 대괄호 사이에 존재하며 문헌의 제목은 쉼표로 구분되어 있습니다.

<br />

- 참조 문헌 목록은 `[ title1, title2, ... , title_n ]` 형식으로 주어집니다. 공백과 쉼표의 위치와 형식은 항상 일정합니다.

<br />

참조 문헌의 제목은 알파벳 소문자와 언더바(\_)로만 구성되어 있으며 10글자를 넘지 않습니다. 또한 참조 문헌은 항상 1개 이상, 10개 이하가 존재합니다. 한 대괄호 내에서 같은 문헌이 중복되어 참조되는 경우는 입력으로 주어지지 않습니다.

<br />

#### 출력 형식

첫 줄에는 수정된 논문을 출력합니다. 문제의 내용과 입/출력 예제를 참고합니다.

이후 한 줄에 하나씩 참조된 순서대로 번호를 부여한 문헌의 목록을 출력합니다.

- 문헌의 번호와 제목을 `[%d] %s` 형식으로 출력합니다

---
