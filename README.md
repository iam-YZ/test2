{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "DeepFaceLab_Colab_V4.ipynb",
      "provenance": [],
      "collapsed_sections": [],
      "toc_visible": true,
      "include_colab_link": true
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "accelerator": "GPU"
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "view-in-github",
        "colab_type": "text"
      },
      "source": [
        "<a href=\"https://colab.research.google.com/github/dream80/DeepFaceLab_Colab/blob/master/DeepFaceLab_Colab_V4.ipynb\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "KWQMLYlFrTXz",
        "colab_type": "text"
      },
      "source": [
        "# 简介\n",
        "\n",
        "无需多说，一路运行即可！\n",
        "\n",
        "此脚本由**[托尼是塔克](https://wxp123.me)**创建，仅用于学习，请勿滥用。如有疑问，拉到最后！"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "AmGMbWZef1XI",
        "colab_type": "code",
        "cellView": "form",
        "colab": {}
      },
      "source": [
        "#@title 查看分配到的设备\n",
        "# 查看设备，是K80还是T4,如果是K80...！\n",
        "! /opt/bin/nvidia-smi"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "ldpC3PjRrYYh",
        "colab_type": "text"
      },
      "source": [
        "# 第一步 准备好workspace\n",
        "这一步你可以选着两种方式\n",
        "1. 使用默认的workspace，你无需自己上传，仅用于熟悉操作。\n",
        "2. 通过Google Drive （谷歌云盘）上传自己的workspace到指定目录。\n",
        "\n",
        "\n",
        "\n",
        "谷歌云盘地址：https://drive.google.com/drive/my-drive  \n",
        "注意：谷歌网址现在都需要“科学上网”才能访问。  "
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "D7toZxhT4J9W",
        "colab_type": "code",
        "cellView": "form",
        "colab": {}
      },
      "source": [
        "#@title 开始挂载 \n",
        "#挂载谷歌云盘\n",
        "#点击链接授权，复制授权码，填入框框，然后回车。\n",
        "\n",
        "from google.colab import drive\n",
        "drive.mount('/content/drive', force_remount=True)\n",
        "\n",
        "#创建DeepFaceLab目录，并且进入目录\n",
        "%cd /content/drive/My Drive/\n",
        "!mkdir DeepFaceLab\n",
        "%cd /content/drive/My Drive/DeepFaceLab/\n"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "_nyimmWVKk8V",
        "colab_type": "text"
      },
      "source": [
        "这一环节有两个选择：  \n",
        "如果你第一次使用，可以加载示例项目，然后往后运行。  \n",
        "如果你是二次运行，就不需要加载了。  \n",
        "如果你希望上传本地素材，可以先用pack脚本把素材打包成faceset.pak到aligned目录，无须解压，可直接进行训练和转换，加载速度会很快。\n"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "cNsq0I98KvHl",
        "colab_type": "code",
        "cellView": "form",
        "colab": {}
      },
      "source": [
        "#@title 加载示例项目workspace \n",
        "# 1. 练手可以使用这一行，直接git clone一个workspace\n",
        "%cd /content/drive/My Drive/DeepFaceLab/\n",
        "!rm -rf workspace\n",
        "!git clone https://github.com/dream80/DFLWorkspace.git workspace"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "yA4nziLPtAf9",
        "colab_type": "text"
      },
      "source": [
        "#第二步 安装DeepFaceLab\n",
        "获取源代码，安装依赖,根据自己的情况选择版本  \n",
        "\n",
        "last:最新版  "
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "4CbWbLzHzqTQ",
        "colab_type": "code",
        "cellView": "form",
        "colab": {}
      },
      "source": [
        "#@title 开始安装\n",
        "\n",
        "Version = \"v2.06.01\" #@param [\"v2.02.03\", \"v2.02.23b\", \"v2.02.28\",\"v2.03.07\",\"v2.03.15\",\"v2.06.01\",\"v2.06.19\",\"last\"]\n",
        "%cd /content/drive/My Drive/DeepFaceLab\n",
        "!rm -fr DeepFaceLab_Colab\n",
        "!pip uninstall -y tensorflow\n",
        "if Version==\"620\":\n",
        "  print(\"620版加载中....\")\n",
        "  # 获取DFL源代码v1.6.1稳定版\n",
        "  !git clone -b v1.6.1 https://github.com/dream80/DeepFaceLab_Colab.git\n",
        "  %cd /content/drive/My Drive/DeepFaceLab/DeepFaceLab_Colab\n",
        "  !pip install -r requirements-colab.txt  \n",
        "elif Version==\"last\":\n",
        "  print(\"最新版加载中....\")\n",
        "  !git clone https://github.com/iperov/DeepFaceLab.git  DeepFaceLab_Colab\n",
        "  %cd /content/drive/My Drive/DeepFaceLab/DeepFaceLab_Colab\n",
        "  !pip install -r requirements-colab.txt\n",
        "else:\n",
        "  print(Version+\"加载中....\")\n",
        "  cmd=\"clone -b \"+Version+\" https://github.com/dream80/DeepFaceLab_Colab.git\"  \n",
        "  !git $cmd\n",
        "  %cd /content/drive/My Drive/DeepFaceLab/DeepFaceLab_Colab\n",
        "  !pip install -r requirements-colab.txt\n",
        "\n",
        "!pip install --upgrade scikit-image\n",
        "!sudo apt-get install cuda-10-0\n"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "9G9s5gJrty-x",
        "colab_type": "text"
      },
      "source": [
        "# 第三步. 提取脸部"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "llMCZb-uMuD1",
        "colab_type": "text"
      },
      "source": [
        "这个步骤其实是分两次的。第一次选src，第二次选dst，很多对Deepfacelab不了解的人可能只点了一个，后面就会报错了。  \n",
        "  \n",
        "这一步，其实有三个小步骤，分解视频，提取人脸，打包素材。 如果有需要可以将他们拆分开来。一般是不用这么做的。 \n",
        "\n",
        "\n",
        "\n"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "6flnCGqpIeen",
        "colab_type": "text"
      },
      "source": [
        ""
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "YnhZyEERAW_D",
        "colab_type": "code",
        "cellView": "form",
        "colab": {}
      },
      "source": [
        "#@title 开始提取\n",
        "target = \"src\" #@param [\"src\",\"dst\"]\n",
        "%cd /content/drive/My Drive/DeepFaceLab/DeepFaceLab_Colab\n",
        "if target==\"src\":\n",
        "  !python main.py videoed extract-video --input-file ../workspace/data_src.mp4 --output-dir ../workspace/data_src/\n",
        "  !python main.py extract --input-dir ../workspace/data_src --output-dir ../workspace/data_src/aligned --detector s3fd\n",
        "  !python main.py util --input-dir ../workspace/data_src/aligned  --pack-faceset\n",
        "else:\n",
        "  !python main.py videoed extract-video --input-file ../workspace/data_dst.mp4 --output-dir ../workspace/data_dst/\n",
        "  !python main.py extract --input-dir ../workspace/data_dst --output-dir ../workspace/data_dst/aligned --detector s3fd --output-debug\n",
        "  !python main.py util --input-dir ../workspace/data_dst/aligned  --pack-faceset\n"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "5MrM8JfIFiy7",
        "colab_type": "code",
        "cellView": "form",
        "colab": {}
      },
      "source": [
        "#@title 开始排序\n",
        "target = \"src\" #@param [\"src\",\"dst\"]\n",
        "\n",
        "if target==\"src\":\n",
        "  #Src排序，可以通过谷歌云盘查看结果，删除不良图片\n",
        "  cmd = \"main.py sort --input-dir ../workspace/data_src/aligned\"\n",
        "  \n",
        "else:\n",
        "  cmd = \"main.py sort --input-dir ../workspace/data_dst/aligned \"\n",
        "\n",
        "!python $cmd"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "QxO1kNO_uRTF",
        "colab_type": "text"
      },
      "source": [
        "# 第四步. 训练模型\n",
        "\n",
        "\n",
        "*  支持SAEHD,Quick96等模型，根据自己的情况选择模型。\n",
        "*  第一次需要配置模型参数，不懂的直接回车默认。\n",
        "*  不想训练了可以点击停止，停止时会抛出异常，但是没什么关系。下次可以继续训练"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "MKQbOrIiyFJL",
        "colab_type": "code",
        "cellView": "form",
        "colab": {}
      },
      "source": [
        "#@title 开始训练\n",
        "Model = \"SAEHD\" #@param [\"SAEHD\",\"Quick96\"]\n",
        "\n",
        "%cd \"/content/drive/My Drive/DeepFaceLab/DeepFaceLab_Colab/\"\n",
        "cmd = \"main.py train --training-data-src-dir ../workspace/data_src/aligned --training-data-dst-dir ../workspace/data_dst/aligned --model-dir ../workspace/model --model \"+Model+\" --no-preview\"\n",
        "!python $cmd"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "oVfZpBR00gBG",
        "colab_type": "text"
      },
      "source": [
        "**模型预览方法请参考：**\n",
        "https://www.wxp123.me/p/329\n",
        "\n",
        "简单描述：左侧->文件->/driver/..../model/ *preview*.jpg->双击  \n",
        "  \n",
        "这种方法比在脚本中预览和在网盘预览都要方便，并且无需启动预览配置！"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "m2Ba96cVuS0K",
        "colab_type": "text"
      },
      "source": [
        "# 第五步. 转换输出\n",
        "\n",
        "训练模型的时候选了什么，这里就选什么  \n",
        "比如训练H128,这里就选H128\n"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "G_oEgeU7I9jt",
        "colab_type": "code",
        "cellView": "form",
        "colab": {}
      },
      "source": [
        "#@title 开始转换图片\n",
        "\n",
        "Model = \"SAEHD\" #@param [\"SAEHD\", \"Quick96\"]\n",
        "%cd \"/content/drive/My Drive/DeepFaceLab/DeepFaceLab_Colab/\"\n",
        "cmd = \" main.py merge --output-mask-dir ../workspace/data_dst/merged_mask --input-dir ../workspace/data_dst --output-dir ../workspace/data_dst/merged --aligned-dir ../workspace/data_dst/aligned --model-dir ../workspace/model --model \" + Model\n",
        "!python $cmd"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab_type": "code",
        "id": "fbxfQ8UbJrqk",
        "cellView": "form",
        "colab": {}
      },
      "source": [
        "#@title 开始图片转视频\n",
        "!python main.py videoed video-from-sequence --input-dir ../workspace/data_dst/merged --output-file ../workspace/result.mp4 --reference-file ../workspace/data_dst.mp4 --include-audio\n",
        "!python main.py videoed video-from-sequence --input-dir ../workspace/data_dst/merged_mask --output-file ../workspace/result_mask.mp4 --reference-file ../workspace/data_dst.mp4 --include-audio --lossless"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "cVfp8xbY5ykL",
        "colab_type": "text"
      },
      "source": [
        "#第六步. 继续训练\n",
        "当你第二次开始训练，或者掉线之后继续训练时并不需要执行上面所有的步骤。只需要下面简单的几个步骤。\n",
        "1. 挂载云盘\n",
        "2. 安装依赖\n",
        "3. 开始训练  \n"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "veL_8afb6WcP",
        "colab_type": "code",
        "cellView": "form",
        "colab": {}
      },
      "source": [
        "#@title 一键运行\n",
        "#挂载谷歌云盘\n",
        "#点击链接授权，复制授权码，填入框框，然后回车。\n",
        "\n",
        "Model = \"SAEHD\" #@param [\"SAEHD\" , \"Quick96\"]\n",
        "\n",
        "\n",
        "from google.colab import drive\n",
        "drive.mount('/content/drive', force_remount=True)\n",
        "\n",
        "\n",
        "# 进入DeepFaceLab_Colab目录\n",
        "%cd /content/drive/My Drive/DeepFaceLab/DeepFaceLab_Colab\n",
        "\n",
        "# 安装Python依赖\n",
        "!pip uninstall -y tensorflow\n",
        "!pip install -r requirements-colab.txt\n",
        "!pip install --upgrade scikit-image\n",
        "!sudo apt-get install cuda-10-0\n",
        "\n",
        "# 开始训练SAE ，如果是其他模型，修改后面的参数即可，比如H128。\n",
        "cmd = \"main.py train --training-data-src-dir ../workspace/data_src/aligned --training-data-dst-dir ../workspace/data_dst/aligned --model-dir ../workspace/model --model \"+Model+\" --no-preview\"\n",
        "!python $cmd "
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "siaPGJ1QVacj",
        "colab_type": "text"
      },
      "source": [
        "# 工具\n"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "WGLTllAHYAPJ",
        "colab_type": "text"
      },
      "source": [
        "\n",
        "打包素材。可以加快上传，下载，加载的速度。"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "fphcCxuh9Qfw",
        "colab_type": "code",
        "cellView": "form",
        "colab": {}
      },
      "source": [
        "#@title 素材打包\n",
        "\n",
        "target = \"src\" #@param [\"src\",\"dst\"]\n",
        "%cd /content/drive/My Drive/DeepFaceLab/DeepFaceLab_Colab\n",
        "if target==\"src\":\n",
        "  !python main.py util --input-dir ../workspace/data_src/aligned  --pack-faceset\n",
        "else:\n",
        "  !python main.py util --input-dir ../workspace/data_dst/aligned  --pack-faceset\n",
        " \n",
        "  "
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "bC2CyDDx6siX",
        "colab_type": "code",
        "cellView": "form",
        "colab": {}
      },
      "source": [
        "#@title 素材解包\n",
        "\n",
        "target = \"src\" #@param [\"src\",\"dst\"]\n",
        "%cd /content/drive/My Drive/DeepFaceLab/DeepFaceLab_Colab\n",
        "if target==\"src\":\n",
        "  !python main.py util --input-dir ../workspace/data_src/aligned --unpack-faceset\n",
        "else:\n",
        "  !python main.py util --input-dir ../workspace/data_dst/aligned --unpack-faceset\n",
        " \n",
        "  "
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "NYDWkORS6xum",
        "colab_type": "code",
        "cellView": "form",
        "colab": {}
      },
      "source": [
        "#@title 素材增强\n",
        "\n",
        "target = \"src\" #@param [\"src\"]\n",
        "%cd /content/drive/My Drive/DeepFaceLab/DeepFaceLab_Colab\n",
        "if target==\"src\":\n",
        "  !python main.py facesettool enhance --input-dir ../workspace/data_src/aligned\n",
        "else:\n",
        "  !python main.py facesettool enhance --input-dir ../workspace/data_dst/aligned\n",
        " \n",
        "  "
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "5H85SsIW1PI3",
        "colab_type": "text"
      },
      "source": [
        "# 其他\n",
        "\n",
        "  QQ群：659480116  \n",
        "    \n",
        " 谷歌云地址：https://drive.google.com/drive/my-drive\n",
        "\n",
        " 作者邮箱 ：wpgdream@gmail.com\n",
        " \n",
        " 使用教程：https://www.wxp123.me\n",
        " \n",
        " \n",
        "不求别的，只求在github给个小星星^_^\n",
        "\n",
        "https://github.com/dream80/DeepFaceLab_Colab\n",
        "\n",
        "右上角star 谢谢!!!  \n",
        "  \n"
      ]
    }
  ]
}
