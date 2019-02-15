

# SAM Basic

오늘은 SAM에 대해서 다뤄 보겠습니다. 뭐 아직, 배우는 중이라서 '이걸 이렇게 써야 한다'라고 딱 부러지게 말씀드리기는 어렵습니다만, 대충 이런 방식으로 돌아간다. 그런 의미로 받아들이는 것이 더욱 좋을 것 같습니다.

SAM은 AWS-CLI와 더불어 command line에서 손쉽게 lambda를 제어하는 용도로 사용됩니다. 이전에 프로젝트를 진행할 때에는 serverless.io를 이용했습니다만, SAM 또한 여러가지 기능들이 제공되므로 간결하게 사용할 수 있습니다. 

## SAM은 무얼하는가

Lambda는 serverless architecture에 꼭 필요한 요소이자, 궁극적으로는 우리가 컴퓨터를 통해  수행하려는 바를 상당한 수준의 추상화를 제공해 주는 tool입니다. 이렇게 활용 가능성이 무긍하며 상대적으로 엄청 저렴함에도 사용하기가 쉽지 않습니다. 어떤 경우에는 있어서는 물리적인 서버에 terminal로 작업하는게 더욱 편하게 느껴질 때도 있습니다. 이는 Program의 핵심적인 요소인 code 관리가 어렵다는 점으로 이어집니다. 개별적인 function이 하나의 작은 program이라고 할 때, 각각의 program은 어떻게 관리해야 하는가? 이런 질문으로 귀결되는 셈이죠. 또한, 우리가 사용하는 현재의 개발 방식에 따르면, 개개의 program(여기서는 function)들의 총아를 바탕으로 repository를 구성하는데, lamdba는 이런 접근이 썩 적합하지 않아 보입니다. 하나의 프로그램이라고 하기에는 개별적으로 너무 독립되며, 그렇다고 각각을 분리하기에는 관리 비용이 커지게 됩니다. 이러한 상황은 lambda의 단점이자 기피하는 이유가 되었고, 이런 상황을 타개하고자 SAM과 같은 tool 개발되었습니다.


SAM은 단순히 code가 lambda가 되기 까지의 과정을 단순화하는 역활을 합니다. 이를 테면, 파일들을 하나의 묶음으로 만들어 lambda가 사용할 수 있도록 S3에 upload하게 되고, 이를 바탕으로 실제  lambda가 해당 파일을 기준으로 작동되게 합니다. 또한, testing을 가능하게 합니다. Developer Console에서도 testing은 가능하지만, UI를 통한 testing은 처음에는 이용하기 무척 쉽다는 장점이 있으나 반복된 테스트는 불편하기 마련입니다. 반복적인 테스트는 아무래도 command line에서 빠르게 진행시키는 것이 전체적인 개발 속도에 좋습니다.

## 

아닌지 뭔가 어렵다는 것은, 사실 정말로 어렵게 느낄 가능성이 많습니다. Actually, Korean is not proper in Vim. The Vim is designed by Western people like American. As you know, every commands are consisted by English characters, these characters need to input mode in English. When we give any commends, we should change the input mode everytime. It is really annoying almost cases. I recommend another way to use 
