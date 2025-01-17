# ibmproject1
#  Fake-News-Detection-Project
## Project Description
The Fake News Detection system is designed to automatically classify news articles as either real or fake, leveraging machine learning and natural language processing (NLP) techniques. With the rise of misinformation across digital platforms, this project aims to provide a reliable tool to verify the authenticity of news content. The system will analyze textual data from news articles and social media posts, distinguishing between factual and misleading information.
## Key Features
1.Develop a machine learning model that can classify news articles as real or fake.
2.Implement natural language processing (NLP) techniques to preprocess and analyze news text.
3.Build a dataset of labeled news articles (fake vs. real) for training and testing.
4.Create a user-friendly interface to allow users to check the legitimacy of news articles.
## Technologies Used
1.Programming Language: Python
2.Libraries: Scikit-learn, TensorFlow/Keras, NLTK, Pandas
3.Web Development: Flask/Django for the backend, HTML/CSS for frontend
4.Dataset: Fake news datasets from Kaggle, online news sources
50Deployment: Heroku/AWS for hosting the system

## program:

```py{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "fake news code_d.ipynb",
      "provenance": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "code",
      "metadata": {
        "id": "UzRteWfLyDwf"
      },
      "source": [
        "# This Python 3 environment comes with many helpful analytics libraries installed\n",
        "# It is defined by the kaggle/python Docker image: https://github.com/kaggle/docker-python\n",
        "# For example, here's several helpful packages to load\n",
        "\n",
        "import numpy as np # linear algebra\n",
        "import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)\n",
        "\n",
        "# Input data files are available in the read-only \"../input/\" directory\n",
        "# For example, running this (by clicking run or pressing Shift+Enter) will list all files under the input directory\n",
        "\n"
      ],
      "execution_count": 35,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "wdEW4Ow5bSpr"
      },
      "source": [
        "# Modelling Algorithms\n",
        "from sklearn.naive_bayes import MultinomialNB\n",
        "from sklearn.linear_model import LogisticRegression\n",
        "from sklearn.linear_model import PassiveAggressiveClassifier\n",
        "\n",
        "# Modelling Helpers\n",
        "from sklearn.model_selection import train_test_split\n",
        "from sklearn.feature_extraction.text import TfidfVectorizer\n",
        "from sklearn.feature_extraction.text import CountVectorizer\n",
        "from sklearn import metrics\n",
        "\n",
        "# Computations\n",
        "import itertools\n",
        "\n",
        "# Visualization\n",
        "import matplotlib.pyplot as plt"
      ],
      "execution_count": 36,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "PP6IetIfv7pv"
      },
      "source": [
        "test  = pd.read_csv (\"test.csv\")\n",
        "submit  = pd.read_csv (\"submit.csv\")"
      ],
      "execution_count": 37,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "YBLqrTSKxEVg"
      },
      "source": [
        "train = pd.read_csv(\"test.csv\")"
      ],
      "execution_count": 38,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 204
        },
        "id": "T7lxKKMj1czM",
        "outputId": "972a64e9-abbb-49a2-8344-085fb0471018"
      },
      "source": [
        "train.head()"
      ],
      "execution_count": 39,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>id</th>\n",
              "      <th>title</th>\n",
              "      <th>author</th>\n",
              "      <th>text</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>20800</td>\n",
              "      <td>Specter of Trump Loosens Tongues, if Not Purse...</td>\n",
              "      <td>David Streitfeld</td>\n",
              "      <td>PALO ALTO, Calif.  —   After years of scorning...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>20801</td>\n",
              "      <td>Russian warships ready to strike terrorists ne...</td>\n",
              "      <td>NaN</td>\n",
              "      <td>Russian warships ready to strike terrorists ne...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>20802</td>\n",
              "      <td>#NoDAPL: Native American Leaders Vow to Stay A...</td>\n",
              "      <td>Common Dreams</td>\n",
              "      <td>Videos #NoDAPL: Native American Leaders Vow to...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>20803</td>\n",
              "      <td>Tim Tebow Will Attempt Another Comeback, This ...</td>\n",
              "      <td>Daniel Victor</td>\n",
              "      <td>If at first you don’t succeed, try a different...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>20804</td>\n",
              "      <td>Keiser Report: Meme Wars (E995)</td>\n",
              "      <td>Truth Broadcast Network</td>\n",
              "      <td>42 mins ago 1 Views 0 Comments 0 Likes 'For th...</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "      id  ...                                               text\n",
              "0  20800  ...  PALO ALTO, Calif.  —   After years of scorning...\n",
              "1  20801  ...  Russian warships ready to strike terrorists ne...\n",
              "2  20802  ...  Videos #NoDAPL: Native American Leaders Vow to...\n",
              "3  20803  ...  If at first you don’t succeed, try a different...\n",
              "4  20804  ...  42 mins ago 1 Views 0 Comments 0 Likes 'For th...\n",
              "\n",
              "[5 rows x 4 columns]"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 39
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "3U5rzwM2z6w7",
        "outputId": "89ff7b91-5fbf-47e1-d585-76af466b35cd"
      },
      "source": [
        "print(f\"Train Shape : {train.shape}\")\n",
        "print(f\"Test Shape : {test.shape}\")\n",
        "print(f\"Submit Shape : {submit.shape}\")"
      ],
      "execution_count": 40,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Train Shape : (5200, 4)\n",
            "Test Shape : (5200, 4)\n",
            "Submit Shape : (5200, 2)\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "RGGMXSj0z-yA",
        "outputId": "32b26ca4-dc80-47e4-bca5-7890fdbca596"
      },
      "source": [
        "train.info()"
      ],
      "execution_count": 41,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 5200 entries, 0 to 5199\n",
            "Data columns (total 4 columns):\n",
            " #   Column  Non-Null Count  Dtype \n",
            "---  ------  --------------  ----- \n",
            " 0   id      5200 non-null   int64 \n",
            " 1   title   5078 non-null   object\n",
            " 2   author  4697 non-null   object\n",
            " 3   text    5193 non-null   object\n",
            "dtypes: int64(1), object(3)\n",
            "memory usage: 162.6+ KB\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "HYlIaspqz-s4",
        "outputId": "98b0b2a2-a2ad-4441-fb21-9ed9cc6322eb"
      },
      "source": [
        "train.isnull().sum()"
      ],
      "execution_count": 42,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "id          0\n",
              "title     122\n",
              "author    503\n",
              "text        7\n",
              "dtype: int64"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 42
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "vEtMre7pz6Nh",
        "outputId": "3a7ef26e-1a15-4777-af43-472167b19a50"
      },
      "source": [
        "train.dtypes.value_counts()"
      ],
      "execution_count": 43,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "object    3\n",
              "int64     1\n",
              "dtype: int64"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 43
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "NtMz6Lli0C_t"
      },
      "source": [
        "test=test.fillna(' ')\n",
        "train=train.fillna(' ')"
      ],
      "execution_count": 44,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "5umpCfm40C86"
      },
      "source": [
        "# Create a column with all the data available\n",
        "test['total']=test['title']+' '+test['author']+' '+test['text']\n",
        "train['total']=train['title']+' '+train['author']+' '+train['text']"
      ],
      "execution_count": 45,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 408
        },
        "id": "wHCDs1mp0C6B",
        "outputId": "890d0b78-55d3-4545-e31b-49b246ec344b"
      },
      "source": [
        "# Have a glance at our training set\n",
        "train.info()\n",
        "train.head()"
      ],
      "execution_count": 46,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 5200 entries, 0 to 5199\n",
            "Data columns (total 5 columns):\n",
            " #   Column  Non-Null Count  Dtype \n",
            "---  ------  --------------  ----- \n",
            " 0   id      5200 non-null   int64 \n",
            " 1   title   5200 non-null   object\n",
            " 2   author  5200 non-null   object\n",
            " 3   text    5200 non-null   object\n",
            " 4   total   5200 non-null   object\n",
            "dtypes: int64(1), object(4)\n",
            "memory usage: 203.2+ KB\n"
          ],
          "name": "stdout"
        },
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>id</th>\n",
              "      <th>title</th>\n",
              "      <th>author</th>\n",
              "      <th>text</th>\n",
              "      <th>total</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>20800</td>\n",
              "      <td>Specter of Trump Loosens Tongues, if Not Purse...</td>\n",
              "      <td>David Streitfeld</td>\n",
              "      <td>PALO ALTO, Calif.  —   After years of scorning...</td>\n",
              "      <td>Specter of Trump Loosens Tongues, if Not Purse...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>20801</td>\n",
              "      <td>Russian warships ready to strike terrorists ne...</td>\n",
              "      <td></td>\n",
              "      <td>Russian warships ready to strike terrorists ne...</td>\n",
              "      <td>Russian warships ready to strike terrorists ne...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>20802</td>\n",
              "      <td>#NoDAPL: Native American Leaders Vow to Stay A...</td>\n",
              "      <td>Common Dreams</td>\n",
              "      <td>Videos #NoDAPL: Native American Leaders Vow to...</td>\n",
              "      <td>#NoDAPL: Native American Leaders Vow to Stay A...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>20803</td>\n",
              "      <td>Tim Tebow Will Attempt Another Comeback, This ...</td>\n",
              "      <td>Daniel Victor</td>\n",
              "      <td>If at first you don’t succeed, try a different...</td>\n",
              "      <td>Tim Tebow Will Attempt Another Comeback, This ...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>20804</td>\n",
              "      <td>Keiser Report: Meme Wars (E995)</td>\n",
              "      <td>Truth Broadcast Network</td>\n",
              "      <td>42 mins ago 1 Views 0 Comments 0 Likes 'For th...</td>\n",
              "      <td>Keiser Report: Meme Wars (E995) Truth Broadcas...</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "      id  ...                                              total\n",
              "0  20800  ...  Specter of Trump Loosens Tongues, if Not Purse...\n",
              "1  20801  ...  Russian warships ready to strike terrorists ne...\n",
              "2  20802  ...  #NoDAPL: Native American Leaders Vow to Stay A...\n",
              "3  20803  ...  Tim Tebow Will Attempt Another Comeback, This ...\n",
              "4  20804  ...  Keiser Report: Meme Wars (E995) Truth Broadcas...\n",
              "\n",
              "[5 rows x 5 columns]"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 46
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "hMkU_ZNd9qxc"
      },
      "source": [
        ""
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "RAdYCyNQOkCT"
      },
      "source": [
        ""
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "juOiuukbOjrY"
      },
      "source": [
        "Hi WAIT ............\n",
        "\n",
        "This is Just Small Demo. \n",
        "\n",
        "Full Code is long... \n",
        "\n",
        "Full Project is Available with Porject Report and PPT.\n",
        "\n",
        "##Mail me at **Vatshayan007@mail.com** to get this project."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "m42H-lyNOjY1"
      },
      "source": [
        ""
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "_PzDiGs9OjVt"
      },
      "source": [
        ""
      ],
      "execution_count": null,
      "outputs": []
    }
  ]
}

```

## output:
![fake-news-detection](https://github.com/user-attachments/assets/b1c8f45b-a142-4b77-af9d-1fa97db28035)



## Future Work
The Fake News Detection system will help users, journalists, and organizations assess the credibility of news articles, contributing to the fight against misinformation in the media.
Custom User Alerts: Notify users when fake news trends on topics they frequently follow.
Fake News Network Detection: Identify and flag sources and networks responsible for spreading fake news.
## Contributors
1. tharika
2. marino sarisha
3. bhavashree
4. santhana lakshmi

