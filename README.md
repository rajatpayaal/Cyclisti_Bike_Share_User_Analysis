# Ecommerce-basket-purchase-inventory-stock-Analysis
We have a huge amount of data set of customer can buy product form ecommerce websites so have to analysis this data and get some useful information means what the customer fulfilments their needs , or as catch the customer interaction, what they buy next product (oil ,breed , butter , jam, other )this is the problem statement we can solve on this project.
# PURPOSE
__________________________________________________________________________
I have a data set consist multiple customer purchase items on by way of transaction data, so in these recent years, the transaction data have been prevalent as research object in means of discovering new information so , we use this information, as well as consume the purchased inventory and services including in the customer factors which can give a rise to their decisions whether and even-if, to purchase this product. Every customer has different needs and tendency as well as have different behaviors of fulfilling on those things. So, in the event of different behaviors to fulfill their needs, they still share some equality, one of them is want to maximize their satisfactions in consuming a necessary product or service. Of that consumption activity, that can be deduce as to the behavior pattern or habit that the customers do in fulfilling their needs and desires so That behavior can be identified consumer needs (supermarket), so we have to design Association Algorithm they can resolve my problem statement by analysis basket (cart) inventory stocks .
{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "Ecommerce basket purchase inventory stock Analysis",
      "provenance": [],
      "authorship_tag": "ABX9TyOLbkUTx6PWSjwJ596blcYe",
      "include_colab_link": true
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
      "cell_type": "markdown",
      "metadata": {
        "id": "view-in-github",
        "colab_type": "text"
      },
      "source": [
        "<a href=\"https://colab.research.google.com/github/rajatpayaal/Ecommerce-basket-purchase-inventory-stock-Analysis/blob/main/Ecommerce_basket_purchase_inventory_stock_Analysis.ipynb\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "## Ecommerce basket purchase inventory stock **Analysis**\n",
        "## Dataset Description\n",
        "**bold text**\n",
        "* Different products given 7500 transactions over the course of a week at a French retail store.\n",
        "* We have library(**apyori**) to calculate the association rule using Apriori."
      ],
      "metadata": {
        "id": "XJx3yQc4RG47"
      }
    },
    {
      "cell_type": "code",
      "execution_count": 1,
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "S68BlpDlQ0nf",
        "outputId": "6f2ad9d4-e368-444a-93a1-4a912c0e3a75"
      },
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Collecting apyori\n",
            "  Downloading apyori-1.1.2.tar.gz (8.6 kB)\n",
            "Building wheels for collected packages: apyori\n",
            "  Building wheel for apyori (setup.py) ... \u001b[?25l\u001b[?25hdone\n",
            "  Created wheel for apyori: filename=apyori-1.1.2-py3-none-any.whl size=5974 sha256=1da130cf67b568d54090b2f02ae0c487adf9fdf5bea0048fb757ed9b64c8267f\n",
            "  Stored in directory: /root/.cache/pip/wheels/cb/f6/e1/57973c631d27efd1a2f375bd6a83b2a616c4021f24aab84080\n",
            "Successfully built apyori\n",
            "Installing collected packages: apyori\n",
            "Successfully installed apyori-1.1.2\n"
          ]
        }
      ],
      "source": [
        "!pip install apyori\n",
        "import numpy as np\n",
        "import pandas as pd\n",
        "import matplotlib.pyplot as plt\n",
        "from apyori import apriori\n"
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "## Read data and **Display**"
      ],
      "metadata": {
        "id": "hQo_E_eLRgqj"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "store_data = pd.read_csv(\"store_data.csv\", header=None)\n",
        "display(store_data.head())\n",
        "print(store_data.shape)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 302
        },
        "id": "8vkYXUBiRiQI",
        "outputId": "72ffeab1-7e87-415f-8df6-17962da53272"
      },
      "execution_count": 2,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/html": [
              "\n",
              "  <div id=\"df-1823419e-a138-4250-b516-0744e72fcdd7\">\n",
              "    <div class=\"colab-df-container\">\n",
              "      <div>\n",
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
              "      <th>0</th>\n",
              "      <th>1</th>\n",
              "      <th>2</th>\n",
              "      <th>3</th>\n",
              "      <th>4</th>\n",
              "      <th>5</th>\n",
              "      <th>6</th>\n",
              "      <th>7</th>\n",
              "      <th>8</th>\n",
              "      <th>9</th>\n",
              "      <th>10</th>\n",
              "      <th>11</th>\n",
              "      <th>12</th>\n",
              "      <th>13</th>\n",
              "      <th>14</th>\n",
              "      <th>15</th>\n",
              "      <th>16</th>\n",
              "      <th>17</th>\n",
              "      <th>18</th>\n",
              "      <th>19</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>shrimp</td>\n",
              "      <td>almonds</td>\n",
              "      <td>avocado</td>\n",
              "      <td>vegetables mix</td>\n",
              "      <td>green grapes</td>\n",
              "      <td>whole weat flour</td>\n",
              "      <td>yams</td>\n",
              "      <td>cottage cheese</td>\n",
              "      <td>energy drink</td>\n",
              "      <td>tomato juice</td>\n",
              "      <td>low fat yogurt</td>\n",
              "      <td>green tea</td>\n",
              "      <td>honey</td>\n",
              "      <td>salad</td>\n",
              "      <td>mineral water</td>\n",
              "      <td>salmon</td>\n",
              "      <td>antioxydant juice</td>\n",
              "      <td>frozen smoothie</td>\n",
              "      <td>spinach</td>\n",
              "      <td>olive oil</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>burgers</td>\n",
              "      <td>meatballs</td>\n",
              "      <td>eggs</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>chutney</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>turkey</td>\n",
              "      <td>avocado</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>mineral water</td>\n",
              "      <td>milk</td>\n",
              "      <td>energy bar</td>\n",
              "      <td>whole wheat rice</td>\n",
              "      <td>green tea</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "      <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-1823419e-a138-4250-b516-0744e72fcdd7')\"\n",
              "              title=\"Convert this dataframe to an interactive table.\"\n",
              "              style=\"display:none;\">\n",
              "        \n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "       width=\"24px\">\n",
              "    <path d=\"M0 0h24v24H0V0z\" fill=\"none\"/>\n",
              "    <path d=\"M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z\"/><path d=\"M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z\"/>\n",
              "  </svg>\n",
              "      </button>\n",
              "      \n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      flex-wrap:wrap;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "      <script>\n",
              "        const buttonEl =\n",
              "          document.querySelector('#df-1823419e-a138-4250-b516-0744e72fcdd7 button.colab-df-convert');\n",
              "        buttonEl.style.display =\n",
              "          google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "        async function convertToInteractive(key) {\n",
              "          const element = document.querySelector('#df-1823419e-a138-4250-b516-0744e72fcdd7');\n",
              "          const dataTable =\n",
              "            await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                     [key], {});\n",
              "          if (!dataTable) return;\n",
              "\n",
              "          const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "            '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "            + ' to learn more about interactive tables.';\n",
              "          element.innerHTML = '';\n",
              "          dataTable['output_type'] = 'display_data';\n",
              "          await google.colab.output.renderOutput(dataTable, element);\n",
              "          const docLink = document.createElement('div');\n",
              "          docLink.innerHTML = docLinkHtml;\n",
              "          element.appendChild(docLink);\n",
              "        }\n",
              "      </script>\n",
              "    </div>\n",
              "  </div>\n",
              "  "
            ],
            "text/plain": [
              "              0          1           2   ...               17       18         19\n",
              "0         shrimp    almonds     avocado  ...  frozen smoothie  spinach  olive oil\n",
              "1        burgers  meatballs        eggs  ...              NaN      NaN        NaN\n",
              "2        chutney        NaN         NaN  ...              NaN      NaN        NaN\n",
              "3         turkey    avocado         NaN  ...              NaN      NaN        NaN\n",
              "4  mineral water       milk  energy bar  ...              NaN      NaN        NaN\n",
              "\n",
              "[5 rows x 20 columns]"
            ]
          },
          "metadata": {}
        },
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "(7501, 20)\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "## Preprocessing on Data\n",
        "*  Here we need a data in form of list for Apriori Algorithm.**bold text**"
      ],
      "metadata": {
        "id": "umzpWJthRzPw"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "records = []\n",
        "for i in range(1, 7501):\n",
        "    records.append([str(store_data.values[i, j]) for j in range(0, 20)])"
      ],
      "metadata": {
        "id": "ZR1KxVIRR49T"
      },
      "execution_count": 4,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "print(type(records))"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "VKrVZrnwSBKB",
        "outputId": "32dcc62e-d786-41c6-ff3d-121d701f910a"
      },
      "execution_count": 5,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "<class 'list'>\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "## Apriori Algorithm\n",
        "\n",
        "* Now time to apply algorithm on data.\n",
        "* We have provide `min_support`, `min_confidence`, `min_lift`, and `min length` of sample-set for find rule.\n",
        "\n",
        "#### Measure 1: Support.\n",
        "This says how popular an itemset is, as measured by the proportion of transactions in which an itemset appears. In Table 1 below, the support of {apple} is 4 out of 8, or 50%. Itemsets can also contain multiple items. For instance, the support of {apple, beer, rice} is 2 out of 8, or 25%.\n",
        "If you discover that sales of items beyond a certain proportion tend to have a significant impact on your profits, you might consider using that proportion as your support threshold. You may then identify itemsets with support values above this threshold as significant itemsets.\n",
        "\n",
        "#### Measure 2: Confidence. \n",
        "This says how likely item Y is purchased when item X is purchased, expressed as {X -> Y}. This is measured by the proportion of transactions with item X, in which item Y also appears. In Table 1, the confidence of {apple -> beer} is 3 out of 4, or 75%.\n",
        "\n",
        "One drawback of the confidence measure is that it might misrepresent the importance of an association. This is because it only accounts for how popular apples are, but not beers. If beers are also very popular in general, there will be a higher chance that a transaction containing apples will also contain beers, thus inflating the confidence measure. To account for the base popularity of both constituent items, we use a third measure called lift.\n",
        "\n",
        "This says how likely item Y is purchased when item X is purchased, while controlling for how popular item Y is. In Table 1, the lift of {apple -> beer} is 1,which implies no association between items. A lift value greater than 1 means that item Y is likely to be bought if item X is bought, while a value less than 1 means that item Y is unlikely to be bought if item X is bought.\n"
      ],
      "metadata": {
        "id": "_GfBGWRWSJir"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "association_rules = apriori(records, min_support=0.0045, min_confidence=0.2, min_lift=3, min_length=2)\n",
        "association_results = list(association_rules)"
      ],
      "metadata": {
        "id": "fwiX45WASTC-"
      },
      "execution_count": 6,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "## How many relation **derived**"
      ],
      "metadata": {
        "id": "6jvdkrMESZI5"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "print(\"There are {} Relation derived.\".format(len(association_results)))"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "OSHtmIwSSbla",
        "outputId": "f7625598-b3d0-4fe2-b86f-0e25de6e07d8"
      },
      "execution_count": 7,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "There are 48 Relation derived.\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "### Association Rules **Derived**"
      ],
      "metadata": {
        "id": "eT6MguWSSf-g"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "for i in range(0, len(association_results)):\n",
        "    print(association_results[i][0])"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "fLX_RJw8Sg6h",
        "outputId": "b54a6f00-5c21-43c9-ad5e-f2bb08572cf8"
      },
      "execution_count": 8,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "frozenset({'chicken', 'light cream'})\n",
            "frozenset({'escalope', 'mushroom cream sauce'})\n",
            "frozenset({'escalope', 'pasta'})\n",
            "frozenset({'herb & pepper', 'ground beef'})\n",
            "frozenset({'tomato sauce', 'ground beef'})\n",
            "frozenset({'whole wheat pasta', 'olive oil'})\n",
            "frozenset({'pasta', 'shrimp'})\n",
            "frozenset({'nan', 'chicken', 'light cream'})\n",
            "frozenset({'chocolate', 'frozen vegetables', 'shrimp'})\n",
            "frozenset({'spaghetti', 'cooking oil', 'ground beef'})\n",
            "frozenset({'escalope', 'nan', 'mushroom cream sauce'})\n",
            "frozenset({'escalope', 'pasta', 'nan'})\n",
            "frozenset({'spaghetti', 'frozen vegetables', 'ground beef'})\n",
            "frozenset({'olive oil', 'milk', 'frozen vegetables'})\n",
            "frozenset({'mineral water', 'frozen vegetables', 'shrimp'})\n",
            "frozenset({'olive oil', 'spaghetti', 'frozen vegetables'})\n",
            "frozenset({'spaghetti', 'frozen vegetables', 'shrimp'})\n",
            "frozenset({'tomatoes', 'spaghetti', 'frozen vegetables'})\n",
            "frozenset({'spaghetti', 'grated cheese', 'ground beef'})\n",
            "frozenset({'herb & pepper', 'mineral water', 'ground beef'})\n",
            "frozenset({'herb & pepper', 'nan', 'ground beef'})\n",
            "frozenset({'spaghetti', 'herb & pepper', 'ground beef'})\n",
            "frozenset({'milk', 'olive oil', 'ground beef'})\n",
            "frozenset({'nan', 'tomato sauce', 'ground beef'})\n",
            "frozenset({'spaghetti', 'shrimp', 'ground beef'})\n",
            "frozenset({'spaghetti', 'milk', 'olive oil'})\n",
            "frozenset({'soup', 'mineral water', 'olive oil'})\n",
            "frozenset({'nan', 'whole wheat pasta', 'olive oil'})\n",
            "frozenset({'pasta', 'nan', 'shrimp'})\n",
            "frozenset({'spaghetti', 'pancakes', 'olive oil'})\n",
            "frozenset({'nan', 'chocolate', 'frozen vegetables', 'shrimp'})\n",
            "frozenset({'spaghetti', 'nan', 'cooking oil', 'ground beef'})\n",
            "frozenset({'spaghetti', 'nan', 'frozen vegetables', 'ground beef'})\n",
            "frozenset({'spaghetti', 'milk', 'frozen vegetables', 'mineral water'})\n",
            "frozenset({'olive oil', 'nan', 'milk', 'frozen vegetables'})\n",
            "frozenset({'nan', 'mineral water', 'frozen vegetables', 'shrimp'})\n",
            "frozenset({'olive oil', 'spaghetti', 'nan', 'frozen vegetables'})\n",
            "frozenset({'spaghetti', 'nan', 'frozen vegetables', 'shrimp'})\n",
            "frozenset({'tomatoes', 'spaghetti', 'nan', 'frozen vegetables'})\n",
            "frozenset({'spaghetti', 'nan', 'grated cheese', 'ground beef'})\n",
            "frozenset({'herb & pepper', 'mineral water', 'nan', 'ground beef'})\n",
            "frozenset({'spaghetti', 'herb & pepper', 'nan', 'ground beef'})\n",
            "frozenset({'nan', 'milk', 'olive oil', 'ground beef'})\n",
            "frozenset({'spaghetti', 'nan', 'shrimp', 'ground beef'})\n",
            "frozenset({'spaghetti', 'nan', 'milk', 'olive oil'})\n",
            "frozenset({'soup', 'nan', 'mineral water', 'olive oil'})\n",
            "frozenset({'spaghetti', 'nan', 'pancakes', 'olive oil'})\n",
            "frozenset({'nan', 'frozen vegetables', 'mineral water', 'spaghetti', 'milk'})\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "## Rules **Generated**"
      ],
      "metadata": {
        "id": "JafyXiiuSlhE"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "for item in association_results:\n",
        "    # first index of the inner list\n",
        "    # Contains base item and add item\n",
        "    pair = item[0]\n",
        "    items = [x for x in pair]\n",
        "    print(\"Rule: \" + items[0] + \" -> \" + items[1])\n",
        "\n",
        "    # second index of the inner list\n",
        "    print(\"Support: \" + str(item[1]))\n",
        "\n",
        "    # third index of the list located at 0th\n",
        "    # of the third index of the inner list\n",
        "\n",
        "    print(\"Confidence: \" + str(item[2][0][2]))\n",
        "    print(\"Lift: \" + str(item[2][0][3]))\n",
        "    print(\"=====================================\")"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "-5z2zJytSndj",
        "outputId": "57593f6b-44aa-4283-b9f6-5f8c8ef779b0"
      },
      "execution_count": 9,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Rule: chicken -> light cream\n",
            "Support: 0.004533333333333334\n",
            "Confidence: 0.2905982905982906\n",
            "Lift: 4.843304843304844\n",
            "=====================================\n",
            "Rule: escalope -> mushroom cream sauce\n",
            "Support: 0.005733333333333333\n",
            "Confidence: 0.30069930069930073\n",
            "Lift: 3.7903273197390845\n",
            "=====================================\n",
            "Rule: escalope -> pasta\n",
            "Support: 0.005866666666666667\n",
            "Confidence: 0.37288135593220345\n",
            "Lift: 4.700185158809287\n",
            "=====================================\n",
            "Rule: herb & pepper -> ground beef\n",
            "Support: 0.016\n",
            "Confidence: 0.3234501347708895\n",
            "Lift: 3.2915549671393096\n",
            "=====================================\n",
            "Rule: tomato sauce -> ground beef\n",
            "Support: 0.005333333333333333\n",
            "Confidence: 0.37735849056603776\n",
            "Lift: 3.840147461662528\n",
            "=====================================\n",
            "Rule: whole wheat pasta -> olive oil\n",
            "Support: 0.008\n",
            "Confidence: 0.2714932126696833\n",
            "Lift: 4.130221288078346\n",
            "=====================================\n",
            "Rule: pasta -> shrimp\n",
            "Support: 0.005066666666666666\n",
            "Confidence: 0.3220338983050848\n",
            "Lift: 4.514493901473151\n",
            "=====================================\n",
            "Rule: nan -> chicken\n",
            "Support: 0.004533333333333334\n",
            "Confidence: 0.2905982905982906\n",
            "Lift: 4.843304843304844\n",
            "=====================================\n",
            "Rule: chocolate -> frozen vegetables\n",
            "Support: 0.005333333333333333\n",
            "Confidence: 0.23255813953488372\n",
            "Lift: 3.260160834601174\n",
            "=====================================\n",
            "Rule: spaghetti -> cooking oil\n",
            "Support: 0.0048\n",
            "Confidence: 0.5714285714285714\n",
            "Lift: 3.281557646029315\n",
            "=====================================\n",
            "Rule: escalope -> nan\n",
            "Support: 0.005733333333333333\n",
            "Confidence: 0.30069930069930073\n",
            "Lift: 3.7903273197390845\n",
            "=====================================\n",
            "Rule: escalope -> pasta\n",
            "Support: 0.005866666666666667\n",
            "Confidence: 0.37288135593220345\n",
            "Lift: 4.700185158809287\n",
            "=====================================\n",
            "Rule: spaghetti -> frozen vegetables\n",
            "Support: 0.008666666666666666\n",
            "Confidence: 0.3110047846889952\n",
            "Lift: 3.164906221394116\n",
            "=====================================\n",
            "Rule: olive oil -> milk\n",
            "Support: 0.0048\n",
            "Confidence: 0.20338983050847456\n",
            "Lift: 3.094165778526489\n",
            "=====================================\n",
            "Rule: mineral water -> frozen vegetables\n",
            "Support: 0.0072\n",
            "Confidence: 0.3068181818181818\n",
            "Lift: 3.2183725365543547\n",
            "=====================================\n",
            "Rule: olive oil -> spaghetti\n",
            "Support: 0.005733333333333333\n",
            "Confidence: 0.20574162679425836\n",
            "Lift: 3.1299436124887174\n",
            "=====================================\n",
            "Rule: spaghetti -> frozen vegetables\n",
            "Support: 0.006\n",
            "Confidence: 0.21531100478468898\n",
            "Lift: 3.0183785717479763\n",
            "=====================================\n",
            "Rule: tomatoes -> spaghetti\n",
            "Support: 0.006666666666666667\n",
            "Confidence: 0.23923444976076555\n",
            "Lift: 3.497579674864993\n",
            "=====================================\n",
            "Rule: spaghetti -> grated cheese\n",
            "Support: 0.005333333333333333\n",
            "Confidence: 0.3225806451612903\n",
            "Lift: 3.282706701098612\n",
            "=====================================\n",
            "Rule: herb & pepper -> mineral water\n",
            "Support: 0.006666666666666667\n",
            "Confidence: 0.390625\n",
            "Lift: 3.975152645861601\n",
            "=====================================\n",
            "Rule: herb & pepper -> nan\n",
            "Support: 0.016\n",
            "Confidence: 0.3234501347708895\n",
            "Lift: 3.2915549671393096\n",
            "=====================================\n",
            "Rule: spaghetti -> herb & pepper\n",
            "Support: 0.0064\n",
            "Confidence: 0.3934426229508197\n",
            "Lift: 4.003825878061259\n",
            "=====================================\n",
            "Rule: milk -> olive oil\n",
            "Support: 0.004933333333333333\n",
            "Confidence: 0.22424242424242424\n",
            "Lift: 3.411395906324912\n",
            "=====================================\n",
            "Rule: nan -> tomato sauce\n",
            "Support: 0.005333333333333333\n",
            "Confidence: 0.37735849056603776\n",
            "Lift: 3.840147461662528\n",
            "=====================================\n",
            "Rule: spaghetti -> shrimp\n",
            "Support: 0.006\n",
            "Confidence: 0.5232558139534884\n",
            "Lift: 3.004914704939635\n",
            "=====================================\n",
            "Rule: spaghetti -> milk\n",
            "Support: 0.0072\n",
            "Confidence: 0.20300751879699247\n",
            "Lift: 3.0883496774390333\n",
            "=====================================\n",
            "Rule: soup -> mineral water\n",
            "Support: 0.0052\n",
            "Confidence: 0.2254335260115607\n",
            "Lift: 3.4295161157945335\n",
            "=====================================\n",
            "Rule: nan -> whole wheat pasta\n",
            "Support: 0.008\n",
            "Confidence: 0.2714932126696833\n",
            "Lift: 4.130221288078346\n",
            "=====================================\n",
            "Rule: pasta -> nan\n",
            "Support: 0.005066666666666666\n",
            "Confidence: 0.3220338983050848\n",
            "Lift: 4.514493901473151\n",
            "=====================================\n",
            "Rule: spaghetti -> pancakes\n",
            "Support: 0.005066666666666666\n",
            "Confidence: 0.20105820105820105\n",
            "Lift: 3.0586947422647217\n",
            "=====================================\n",
            "Rule: nan -> chocolate\n",
            "Support: 0.005333333333333333\n",
            "Confidence: 0.23255813953488372\n",
            "Lift: 3.260160834601174\n",
            "=====================================\n",
            "Rule: spaghetti -> nan\n",
            "Support: 0.0048\n",
            "Confidence: 0.5714285714285714\n",
            "Lift: 3.281557646029315\n",
            "=====================================\n",
            "Rule: spaghetti -> nan\n",
            "Support: 0.008666666666666666\n",
            "Confidence: 0.3110047846889952\n",
            "Lift: 3.164906221394116\n",
            "=====================================\n",
            "Rule: spaghetti -> milk\n",
            "Support: 0.004533333333333334\n",
            "Confidence: 0.28813559322033905\n",
            "Lift: 3.0224013274860737\n",
            "=====================================\n",
            "Rule: olive oil -> nan\n",
            "Support: 0.0048\n",
            "Confidence: 0.20338983050847456\n",
            "Lift: 3.094165778526489\n",
            "=====================================\n",
            "Rule: nan -> mineral water\n",
            "Support: 0.0072\n",
            "Confidence: 0.3068181818181818\n",
            "Lift: 3.2183725365543547\n",
            "=====================================\n",
            "Rule: olive oil -> spaghetti\n",
            "Support: 0.005733333333333333\n",
            "Confidence: 0.20574162679425836\n",
            "Lift: 3.1299436124887174\n",
            "=====================================\n",
            "Rule: spaghetti -> nan\n",
            "Support: 0.006\n",
            "Confidence: 0.21531100478468898\n",
            "Lift: 3.0183785717479763\n",
            "=====================================\n",
            "Rule: tomatoes -> spaghetti\n",
            "Support: 0.006666666666666667\n",
            "Confidence: 0.23923444976076555\n",
            "Lift: 3.497579674864993\n",
            "=====================================\n",
            "Rule: spaghetti -> nan\n",
            "Support: 0.005333333333333333\n",
            "Confidence: 0.3225806451612903\n",
            "Lift: 3.282706701098612\n",
            "=====================================\n",
            "Rule: herb & pepper -> mineral water\n",
            "Support: 0.006666666666666667\n",
            "Confidence: 0.390625\n",
            "Lift: 3.975152645861601\n",
            "=====================================\n",
            "Rule: spaghetti -> herb & pepper\n",
            "Support: 0.0064\n",
            "Confidence: 0.3934426229508197\n",
            "Lift: 4.003825878061259\n",
            "=====================================\n",
            "Rule: nan -> milk\n",
            "Support: 0.004933333333333333\n",
            "Confidence: 0.22424242424242424\n",
            "Lift: 3.411395906324912\n",
            "=====================================\n",
            "Rule: spaghetti -> nan\n",
            "Support: 0.006\n",
            "Confidence: 0.5232558139534884\n",
            "Lift: 3.004914704939635\n",
            "=====================================\n",
            "Rule: spaghetti -> nan\n",
            "Support: 0.0072\n",
            "Confidence: 0.20300751879699247\n",
            "Lift: 3.0883496774390333\n",
            "=====================================\n",
            "Rule: soup -> nan\n",
            "Support: 0.0052\n",
            "Confidence: 0.2254335260115607\n",
            "Lift: 3.4295161157945335\n",
            "=====================================\n",
            "Rule: spaghetti -> nan\n",
            "Support: 0.005066666666666666\n",
            "Confidence: 0.20105820105820105\n",
            "Lift: 3.0586947422647217\n",
            "=====================================\n",
            "Rule: nan -> frozen vegetables\n",
            "Support: 0.004533333333333334\n",
            "Confidence: 0.28813559322033905\n",
            "Lift: 3.0224013274860737\n",
            "=====================================\n"
          ]
        }
      ]
    }
  ]
}
