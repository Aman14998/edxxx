{
    "cells": [
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "<a href=\"https://www.bigdatauniversity.com\"><img src=\"https://ibm.box.com/shared/static/cw2c7r3o20w9zn8gkecaeyjhgw3xdgbj.png\" width=\"400\" align=\"center\"></a>\n\n<h1 align=\"center\"><font size=\"5\">Classification with Python</font></h1>"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "In this notebook we try to practice all the classification algorithms that we learned in this course.\n\nWe load a dataset using Pandas library, and apply the following algorithms, and find the best one for this specific dataset by accuracy evaluation methods.\n\nLets first load required libraries:"
        },
        {
            "cell_type": "code",
            "execution_count": 2,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [],
            "source": "import itertools\nimport numpy as np\nimport matplotlib.pyplot as plt\nfrom matplotlib.ticker import NullFormatter\nimport pandas as pd\nimport numpy as np\nimport matplotlib.ticker as ticker\nfrom sklearn import preprocessing\n%matplotlib inline"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "### About dataset"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "This dataset is about the performance of basketball teams. The __cbb.csv__ data set includes performance data about five seasons of 354 basketball teams. It includes following fields:\n\n| Field          | Description                                                                           |\n|----------------|---------------------------------------------------------------------------------------|\n|TEAM |\tThe Division I college basketball school|\n|CONF|\tThe Athletic Conference in which the school participates in (A10 = Atlantic 10, ACC = Atlantic Coast Conference, AE = America East, Amer = American, ASun = ASUN, B10 = Big Ten, B12 = Big 12, BE = Big East, BSky = Big Sky, BSth = Big South, BW = Big West, CAA = Colonial Athletic Association, CUSA = Conference USA, Horz = Horizon League, Ivy = Ivy League, MAAC = Metro Atlantic Athletic Conference, MAC = Mid-American Conference, MEAC = Mid-Eastern Athletic Conference, MVC = Missouri Valley Conference, MWC = Mountain West, NEC = Northeast Conference, OVC = Ohio Valley Conference, P12 = Pac-12, Pat = Patriot League, SB = Sun Belt, SC = Southern Conference, SEC = South Eastern Conference, Slnd = Southland Conference, Sum = Summit League, SWAC = Southwestern Athletic Conference, WAC = Western Athletic Conference, WCC = West Coast Conference)|\n|G|\tNumber of games played|\n|W|\tNumber of games won|\n|ADJOE|\tAdjusted Offensive Efficiency (An estimate of the offensive efficiency (points scored per 100 possessions) a team would have against the average Division I defense)|\n|ADJDE|\tAdjusted Defensive Efficiency (An estimate of the defensive efficiency (points allowed per 100 possessions) a team would have against the average Division I offense)|\n|BARTHAG|\tPower Rating (Chance of beating an average Division I team)|\n|EFG_O|\tEffective Field Goal Percentage Shot|\n|EFG_D|\tEffective Field Goal Percentage Allowed|\n|TOR|\tTurnover Percentage Allowed (Turnover Rate)|\n|TORD|\tTurnover Percentage Committed (Steal Rate)|\n|ORB|\tOffensive Rebound Percentage|\n|DRB|\tDefensive Rebound Percentage|\n|FTR|\tFree Throw Rate (How often the given team shoots Free Throws)|\n|FTRD|\tFree Throw Rate Allowed|\n|2P_O|\tTwo-Point Shooting Percentage|\n|2P_D|\tTwo-Point Shooting Percentage Allowed|\n|3P_O|\tThree-Point Shooting Percentage|\n|3P_D|\tThree-Point Shooting Percentage Allowed|\n|ADJ_T|\tAdjusted Tempo (An estimate of the tempo (possessions per 40 minutes) a team would have against the team that wants to play at an average Division I tempo)|\n|WAB|\tWins Above Bubble (The bubble refers to the cut off between making the NCAA March Madness Tournament and not making it)|\n|POSTSEASON|\tRound where the given team was eliminated or where their season ended (R68 = First Four, R64 = Round of 64, R32 = Round of 32, S16 = Sweet Sixteen, E8 = Elite Eight, F4 = Final Four, 2ND = Runner-up, Champion = Winner of the NCAA March Madness Tournament for that given year)|\n|SEED|\tSeed in the NCAA March Madness Tournament|\n|YEAR|\tSeason"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "### Load Data From CSV File  "
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "Let's load the dataset [NB Need to provide link to csv file]"
        },
        {
            "cell_type": "code",
            "execution_count": 3,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [
                {
                    "data": {
                        "text/html": "<div>\n<style scoped>\n    .dataframe tbody tr th:only-of-type {\n        vertical-align: middle;\n    }\n\n    .dataframe tbody tr th {\n        vertical-align: top;\n    }\n\n    .dataframe thead th {\n        text-align: right;\n    }\n</style>\n<table border=\"1\" class=\"dataframe\">\n  <thead>\n    <tr style=\"text-align: right;\">\n      <th></th>\n      <th>TEAM</th>\n      <th>CONF</th>\n      <th>G</th>\n      <th>W</th>\n      <th>ADJOE</th>\n      <th>ADJDE</th>\n      <th>BARTHAG</th>\n      <th>EFG_O</th>\n      <th>EFG_D</th>\n      <th>TOR</th>\n      <th>...</th>\n      <th>FTRD</th>\n      <th>2P_O</th>\n      <th>2P_D</th>\n      <th>3P_O</th>\n      <th>3P_D</th>\n      <th>ADJ_T</th>\n      <th>WAB</th>\n      <th>POSTSEASON</th>\n      <th>SEED</th>\n      <th>YEAR</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>0</th>\n      <td>North Carolina</td>\n      <td>ACC</td>\n      <td>40</td>\n      <td>33</td>\n      <td>123.3</td>\n      <td>94.9</td>\n      <td>0.9531</td>\n      <td>52.6</td>\n      <td>48.1</td>\n      <td>15.4</td>\n      <td>...</td>\n      <td>30.4</td>\n      <td>53.9</td>\n      <td>44.6</td>\n      <td>32.7</td>\n      <td>36.2</td>\n      <td>71.7</td>\n      <td>8.6</td>\n      <td>2ND</td>\n      <td>1.0</td>\n      <td>2016</td>\n    </tr>\n    <tr>\n      <th>1</th>\n      <td>Villanova</td>\n      <td>BE</td>\n      <td>40</td>\n      <td>35</td>\n      <td>123.1</td>\n      <td>90.9</td>\n      <td>0.9703</td>\n      <td>56.1</td>\n      <td>46.7</td>\n      <td>16.3</td>\n      <td>...</td>\n      <td>30.0</td>\n      <td>57.4</td>\n      <td>44.1</td>\n      <td>36.2</td>\n      <td>33.9</td>\n      <td>66.7</td>\n      <td>8.9</td>\n      <td>Champions</td>\n      <td>2.0</td>\n      <td>2016</td>\n    </tr>\n    <tr>\n      <th>2</th>\n      <td>Notre Dame</td>\n      <td>ACC</td>\n      <td>36</td>\n      <td>24</td>\n      <td>118.3</td>\n      <td>103.3</td>\n      <td>0.8269</td>\n      <td>54.0</td>\n      <td>49.5</td>\n      <td>15.3</td>\n      <td>...</td>\n      <td>26.0</td>\n      <td>52.9</td>\n      <td>46.5</td>\n      <td>37.4</td>\n      <td>36.9</td>\n      <td>65.5</td>\n      <td>2.3</td>\n      <td>E8</td>\n      <td>6.0</td>\n      <td>2016</td>\n    </tr>\n    <tr>\n      <th>3</th>\n      <td>Virginia</td>\n      <td>ACC</td>\n      <td>37</td>\n      <td>29</td>\n      <td>119.9</td>\n      <td>91.0</td>\n      <td>0.9600</td>\n      <td>54.8</td>\n      <td>48.4</td>\n      <td>15.1</td>\n      <td>...</td>\n      <td>33.4</td>\n      <td>52.6</td>\n      <td>46.3</td>\n      <td>40.3</td>\n      <td>34.7</td>\n      <td>61.9</td>\n      <td>8.6</td>\n      <td>E8</td>\n      <td>1.0</td>\n      <td>2016</td>\n    </tr>\n    <tr>\n      <th>4</th>\n      <td>Kansas</td>\n      <td>B12</td>\n      <td>37</td>\n      <td>32</td>\n      <td>120.9</td>\n      <td>90.4</td>\n      <td>0.9662</td>\n      <td>55.7</td>\n      <td>45.1</td>\n      <td>17.8</td>\n      <td>...</td>\n      <td>37.3</td>\n      <td>52.7</td>\n      <td>43.4</td>\n      <td>41.3</td>\n      <td>32.5</td>\n      <td>70.1</td>\n      <td>11.6</td>\n      <td>E8</td>\n      <td>1.0</td>\n      <td>2016</td>\n    </tr>\n  </tbody>\n</table>\n<p>5 rows \u00d7 24 columns</p>\n</div>",
                        "text/plain": "             TEAM CONF   G   W  ADJOE  ADJDE  BARTHAG  EFG_O  EFG_D   TOR  \\\n0  North Carolina  ACC  40  33  123.3   94.9   0.9531   52.6   48.1  15.4   \n1       Villanova   BE  40  35  123.1   90.9   0.9703   56.1   46.7  16.3   \n2      Notre Dame  ACC  36  24  118.3  103.3   0.8269   54.0   49.5  15.3   \n3        Virginia  ACC  37  29  119.9   91.0   0.9600   54.8   48.4  15.1   \n4          Kansas  B12  37  32  120.9   90.4   0.9662   55.7   45.1  17.8   \n\n   ...  FTRD  2P_O  2P_D  3P_O  3P_D  ADJ_T   WAB  POSTSEASON  SEED  YEAR  \n0  ...  30.4  53.9  44.6  32.7  36.2   71.7   8.6         2ND   1.0  2016  \n1  ...  30.0  57.4  44.1  36.2  33.9   66.7   8.9   Champions   2.0  2016  \n2  ...  26.0  52.9  46.5  37.4  36.9   65.5   2.3          E8   6.0  2016  \n3  ...  33.4  52.6  46.3  40.3  34.7   61.9   8.6          E8   1.0  2016  \n4  ...  37.3  52.7  43.4  41.3  32.5   70.1  11.6          E8   1.0  2016  \n\n[5 rows x 24 columns]"
                    },
                    "execution_count": 3,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "df = pd.read_csv('https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/ML0120ENv3/Dataset/ML0101EN_EDX_skill_up/cbb.csv')\ndf.head()"
        },
        {
            "cell_type": "code",
            "execution_count": 4,
            "metadata": {},
            "outputs": [
                {
                    "data": {
                        "text/plain": "(1406, 24)"
                    },
                    "execution_count": 4,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "df.shape"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "## Add Column\nNext we'll add a column that will contain \"true\" if the wins above bubble are over 7 and \"false\" if not. We'll call this column Win Index or \"windex\" for short. "
        },
        {
            "cell_type": "code",
            "execution_count": 5,
            "metadata": {},
            "outputs": [],
            "source": "df['windex'] = np.where(df.WAB > 7, 'True', 'False')"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "# Data visualization and pre-processing\n\n"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "Next we'll filter the data set to the teams that made the Sweet Sixteen, the Elite Eight, and the Final Four in the post season. We'll also create a new dataframe that will hold the values with the new column."
        },
        {
            "cell_type": "code",
            "execution_count": 6,
            "metadata": {},
            "outputs": [
                {
                    "data": {
                        "text/html": "<div>\n<style scoped>\n    .dataframe tbody tr th:only-of-type {\n        vertical-align: middle;\n    }\n\n    .dataframe tbody tr th {\n        vertical-align: top;\n    }\n\n    .dataframe thead th {\n        text-align: right;\n    }\n</style>\n<table border=\"1\" class=\"dataframe\">\n  <thead>\n    <tr style=\"text-align: right;\">\n      <th></th>\n      <th>TEAM</th>\n      <th>CONF</th>\n      <th>G</th>\n      <th>W</th>\n      <th>ADJOE</th>\n      <th>ADJDE</th>\n      <th>BARTHAG</th>\n      <th>EFG_O</th>\n      <th>EFG_D</th>\n      <th>TOR</th>\n      <th>...</th>\n      <th>2P_O</th>\n      <th>2P_D</th>\n      <th>3P_O</th>\n      <th>3P_D</th>\n      <th>ADJ_T</th>\n      <th>WAB</th>\n      <th>POSTSEASON</th>\n      <th>SEED</th>\n      <th>YEAR</th>\n      <th>windex</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>2</th>\n      <td>Notre Dame</td>\n      <td>ACC</td>\n      <td>36</td>\n      <td>24</td>\n      <td>118.3</td>\n      <td>103.3</td>\n      <td>0.8269</td>\n      <td>54.0</td>\n      <td>49.5</td>\n      <td>15.3</td>\n      <td>...</td>\n      <td>52.9</td>\n      <td>46.5</td>\n      <td>37.4</td>\n      <td>36.9</td>\n      <td>65.5</td>\n      <td>2.3</td>\n      <td>E8</td>\n      <td>6.0</td>\n      <td>2016</td>\n      <td>False</td>\n    </tr>\n    <tr>\n      <th>3</th>\n      <td>Virginia</td>\n      <td>ACC</td>\n      <td>37</td>\n      <td>29</td>\n      <td>119.9</td>\n      <td>91.0</td>\n      <td>0.9600</td>\n      <td>54.8</td>\n      <td>48.4</td>\n      <td>15.1</td>\n      <td>...</td>\n      <td>52.6</td>\n      <td>46.3</td>\n      <td>40.3</td>\n      <td>34.7</td>\n      <td>61.9</td>\n      <td>8.6</td>\n      <td>E8</td>\n      <td>1.0</td>\n      <td>2016</td>\n      <td>True</td>\n    </tr>\n    <tr>\n      <th>4</th>\n      <td>Kansas</td>\n      <td>B12</td>\n      <td>37</td>\n      <td>32</td>\n      <td>120.9</td>\n      <td>90.4</td>\n      <td>0.9662</td>\n      <td>55.7</td>\n      <td>45.1</td>\n      <td>17.8</td>\n      <td>...</td>\n      <td>52.7</td>\n      <td>43.4</td>\n      <td>41.3</td>\n      <td>32.5</td>\n      <td>70.1</td>\n      <td>11.6</td>\n      <td>E8</td>\n      <td>1.0</td>\n      <td>2016</td>\n      <td>True</td>\n    </tr>\n    <tr>\n      <th>5</th>\n      <td>Oregon</td>\n      <td>P12</td>\n      <td>37</td>\n      <td>30</td>\n      <td>118.4</td>\n      <td>96.2</td>\n      <td>0.9163</td>\n      <td>52.3</td>\n      <td>48.9</td>\n      <td>16.1</td>\n      <td>...</td>\n      <td>52.6</td>\n      <td>46.1</td>\n      <td>34.4</td>\n      <td>36.2</td>\n      <td>69.0</td>\n      <td>6.7</td>\n      <td>E8</td>\n      <td>1.0</td>\n      <td>2016</td>\n      <td>False</td>\n    </tr>\n    <tr>\n      <th>6</th>\n      <td>Syracuse</td>\n      <td>ACC</td>\n      <td>37</td>\n      <td>23</td>\n      <td>111.9</td>\n      <td>93.6</td>\n      <td>0.8857</td>\n      <td>50.0</td>\n      <td>47.3</td>\n      <td>18.1</td>\n      <td>...</td>\n      <td>47.2</td>\n      <td>48.1</td>\n      <td>36.0</td>\n      <td>30.7</td>\n      <td>65.5</td>\n      <td>-0.3</td>\n      <td>F4</td>\n      <td>10.0</td>\n      <td>2016</td>\n      <td>False</td>\n    </tr>\n  </tbody>\n</table>\n<p>5 rows \u00d7 25 columns</p>\n</div>",
                        "text/plain": "         TEAM CONF   G   W  ADJOE  ADJDE  BARTHAG  EFG_O  EFG_D   TOR  ...  \\\n2  Notre Dame  ACC  36  24  118.3  103.3   0.8269   54.0   49.5  15.3  ...   \n3    Virginia  ACC  37  29  119.9   91.0   0.9600   54.8   48.4  15.1  ...   \n4      Kansas  B12  37  32  120.9   90.4   0.9662   55.7   45.1  17.8  ...   \n5      Oregon  P12  37  30  118.4   96.2   0.9163   52.3   48.9  16.1  ...   \n6    Syracuse  ACC  37  23  111.9   93.6   0.8857   50.0   47.3  18.1  ...   \n\n   2P_O  2P_D  3P_O  3P_D  ADJ_T   WAB  POSTSEASON  SEED  YEAR  windex  \n2  52.9  46.5  37.4  36.9   65.5   2.3          E8   6.0  2016   False  \n3  52.6  46.3  40.3  34.7   61.9   8.6          E8   1.0  2016    True  \n4  52.7  43.4  41.3  32.5   70.1  11.6          E8   1.0  2016    True  \n5  52.6  46.1  34.4  36.2   69.0   6.7          E8   1.0  2016   False  \n6  47.2  48.1  36.0  30.7   65.5  -0.3          F4  10.0  2016   False  \n\n[5 rows x 25 columns]"
                    },
                    "execution_count": 6,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "df1 = df[df['POSTSEASON'].str.contains('F4|S16|E8', na=False)]\ndf1.head()"
        },
        {
            "cell_type": "code",
            "execution_count": 7,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [
                {
                    "data": {
                        "text/plain": "S16    32\nE8     16\nF4      8\nName: POSTSEASON, dtype: int64"
                    },
                    "execution_count": 7,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "df1['POSTSEASON'].value_counts()"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "32 teams made it into the Sweet Sixteen, 16 into the Elite Eight, and 8 made it into the Final Four over 5 seasons. \n"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "Lets plot some columns to underestand data better:"
        },
        {
            "cell_type": "code",
            "execution_count": 8,
            "metadata": {},
            "outputs": [
                {
                    "name": "stdout",
                    "output_type": "stream",
                    "text": "Solving environment: done\n\n## Package Plan ##\n\n  environment location: /opt/conda/envs/Python36\n\n  added / updated specs: \n    - seaborn\n\n\nThe following packages will be downloaded:\n\n    package                    |            build\n    ---------------------------|-----------------\n    openssl-1.1.1g             |       h7b6447c_0         3.8 MB  anaconda\n    seaborn-0.10.0             |             py_0         161 KB  anaconda\n    ca-certificates-2020.1.1   |                0         132 KB  anaconda\n    certifi-2020.4.5.1         |           py36_0         159 KB  anaconda\n    ------------------------------------------------------------\n                                           Total:         4.2 MB\n\nThe following packages will be UPDATED:\n\n    ca-certificates: 2020.1.1-0         --> 2020.1.1-0        anaconda\n    certifi:         2020.4.5.1-py36_0  --> 2020.4.5.1-py36_0 anaconda\n    openssl:         1.1.1g-h7b6447c_0  --> 1.1.1g-h7b6447c_0 anaconda\n    seaborn:         0.9.0-pyh91ea838_1 --> 0.10.0-py_0       anaconda\n\n\nDownloading and Extracting Packages\nopenssl-1.1.1g       | 3.8 MB    | ##################################### | 100% \nseaborn-0.10.0       | 161 KB    | ##################################### | 100% \nca-certificates-2020 | 132 KB    | ##################################### | 100% \ncertifi-2020.4.5.1   | 159 KB    | ##################################### | 100% \nPreparing transaction: done\nVerifying transaction: done\nExecuting transaction: done\n"
                }
            ],
            "source": "# notice: installing seaborn might takes a few minutes\n!conda install -c anaconda seaborn -y"
        },
        {
            "cell_type": "code",
            "execution_count": 9,
            "metadata": {},
            "outputs": [
                {
                    "data": {
                        "image/png": "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\n",
                        "text/plain": "<Figure size 1296x216 with 2 Axes>"
                    },
                    "metadata": {
                        "needs_background": "light"
                    },
                    "output_type": "display_data"
                }
            ],
            "source": "import seaborn as sns\n\nbins = np.linspace(df1.BARTHAG.min(), df1.BARTHAG.max(), 10)\ng = sns.FacetGrid(df1, col=\"windex\", hue=\"POSTSEASON\", palette=\"Set1\", col_wrap=6)\ng.map(plt.hist, 'BARTHAG', bins=bins, ec=\"k\")\n\ng.axes[-1].legend()\nplt.show()"
        },
        {
            "cell_type": "code",
            "execution_count": 10,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [
                {
                    "data": {
                        "image/png": "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kggoAECSCCgAQJIIKABAkggoAECSCCgAQJIIKABAkggoAECSCCgAQJIIKABAkggoAECSCCgAQJIIKABAkggoAECSCCgAQJIIKABAkggoAECSCCgAQJIIKABAkggoAECSeg0o29fYfsH2soEoCAAAqbwzqGsldfZzHQAAvEWvARUR90v64wDUAgDADkPy2pDt6ZKmS1JbW9su27a0tah7VXdeu85NXUOdbFe6DNSIvvSZlOXZn5tHNev5lc/nsq2Uf8+0dnclt60U5RZQETFb0mxJ6ujoiF217V7VrWNunpLLfm+bOjeX7UjSti3bkqwL1akvfSZlqfbnVOvK+/dMiseYF67iAwAkiYACACSpnMvMfy7pQUkfsN1l+5T+LwsAUOt6/RtURJw4EIUAAFCMIT4AQJIIKABAkggoAECSCCgAQJIIKABAkggoAECSCCgAQJIIKABAkggoAECSCCgAQJIIKABAkggoAECSCCgAQJIIKABAkggoAECSCCgAQJIIKABAkggoAECSCCgAQJIIKABAkggoAECSCCgAQJIIKABAkggoAECSCCgAQJIIKABAkggoAECSygoo2522n7T9tO1v93dRAAD0GlC26yVdKekoSQdKOtH2gf1dGACgtpVzBvVhSU9HxDMR8YakGyUd279lAQBqnSNi1w3sz0nqjIhTs/tfljQpIs7Yqd10SdOzux+Q9GT+5Q6IEZLWVbqId6gajkEaHMexLiI6d+eJVdRnpMHxWvWGYxg4ZfWbIWVsyCXWvS3VImK2pNllbC9pth+JiI5K1/FOVMMxSNVzHD2plj4jVcdrxTGkp5whvi5Jo4rut0rq7p9yAAAoKCegfifp/bbH2N5D0gmSftW/ZQEAal2vQ3wRsdX2GZLulFQv6ZqIeLzfK6ucahhyqYZjkKrnOGpBNbxWHENier1IAgCASmAmCQBAkggoAECSai6gbF9j+wXby4rWfd7247a32e7Yqf252RRPT9r+9MBX/HZ9OQbbo21vtr0kW35UmarfqodjuNT2720/Zvtm23sXPZbc61BL6Df0m4qIiJpaJB0h6VBJy4rWjVPhg5LzJHUUrT9Q0lJJQyWNkfQHSfWD7BhGF7dLZenhGI6UNCS7fbGki1N+HWppod+ksdRav6m5M6iIuF/SH3datzwiSn2K/1hJN0bE6xHxrKSnVZj6qaL6eAxJ6uEY7oqIrdndh1T4zJ2U6OtQS+g3aai1flNzAdVHLZJWFd3vytYNNmNsP2r7Ptsfr3QxZfpbSb/JblfL61ArquX1ot9UWDlTHdWysqZ5StxqSW0Rsd72REm32B4fES9VurCe2D5P0lZJN2xfVaLZYHsdakk1vF70mwRwBrVrg36ap+z0fn12e5EK49BjK1tVz2z/jaSjJZ0U2UC6quB1qDGD/vWi36SBgNq1X0k6wfZQ22MkvV/SwxWuqU9svyv7Ti/Z3l+FY3imslWVZrtT0rckfSYiXi16aNC/DjVm0L9e9JtEVPoqjYFeJP1chdP3LSq8wzhF0tTs9uuS/k/SnUXtz1Ph3dOTko6qdP19PQZJx0t6XIWreRZLOqbS9e/iGJ5WYcx8Sbb8KOXXoZYW+g39phILUx0BAJLEEB8AIEkEFAAgSQQUACBJBBQAIEkEFAAgSQRUgmxPtR22P5jd3z6z8qO2l9t+OPtg3vb2J9u+ouj+9Gx2499nbQ8vemxeNrPx9lmabxrYowPyR5+pTkx1lKYTJT0g6QRJF2Tr/hARh0g7Pjj4S9t1EfGT4ifaPlrSVyUdHhHrbB+qwjQtH46INVmzkyLikYE4EGCA0GeqEGdQibHdJOljKnwA74RSbSLiGUlnS5pR4uFvSZoZEeuytoslXSfpa/1SMFBh9JnqRUCl57OS7oiIFZL+mL2bK2WxpA+WWD9e0qKd1j2Srd/uhqLhikvfccVAZdFnqhRDfOk5UdL3sts3ZvevLNGu1EzFPbHeOosxwxWoJvSZKkVAJcT2vpI+Kekg2yGpXoVOclWJ5odIWl5i/ROSJkr6bdG6Q7P1QFWhz1Q3hvjS8jlJ10fEfhExOiJGSXpWf/qGTEmFK5QkXSbpByW2cYmki7OOK9vtkk5W6Q4LDHb0mSrGGVRaTpQ0a6d1v5D0j5LeZ/tRSY2SXpb0g6KrkYaoMBuzIuJXtlskLcjeUb4s6UsRsbpomzfY3pzdXhcRn+qfwwH6HX2mijGbeRWwfbmkpyKCd3xAGegzgwMBNcjZ/o2kPSQdFxEbK10PkDr6zOBBQAEAksRFEgCAJBFQAIAkEVAAgCQRUACAJBFQAIAk/T9mzBRRZdMnhAAAAABJRU5ErkJggg==\n",
                        "text/plain": "<Figure size 432x216 with 2 Axes>"
                    },
                    "metadata": {
                        "needs_background": "light"
                    },
                    "output_type": "display_data"
                }
            ],
            "source": "bins = np.linspace(df1.ADJOE.min(), df1.ADJOE.max(), 10)\ng = sns.FacetGrid(df1, col=\"windex\", hue=\"POSTSEASON\", palette=\"Set1\", col_wrap=2)\ng.map(plt.hist, 'ADJOE', bins=bins, ec=\"k\")\n\ng.axes[-1].legend()\nplt.show()"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "# Pre-processing:  Feature selection/extraction"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "### Lets look at how Adjusted Defense Efficiency plots"
        },
        {
            "cell_type": "code",
            "execution_count": 11,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [
                {
                    "data": {
                        "image/png": "iVBORw0KGgoAAAANSUhEUgAAAagAAADQCAYAAABStPXYAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDMuMC4yLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvOIA7rQAAFSxJREFUeJzt3X2UXHV9x/H3Z5OQDVkQTECzuywJTxoe4gJr4wNCCpaGFAoL1IAPFY9KK4IarFRqK1jlHBRaLPJwGiIERLGnaEQg5aFQIFaIJrBBIBIQaLJsIkkk4RkS8u0fcxOHZSY7G+7s/Gb28zpnzs7c+c3vfu9kfvnM/c3MvYoIzMzMUtNU6wLMzMxKcUCZmVmSHFBmZpYkB5SZmSXJAWVmZklyQJmZWZIcUFUgab6knQbRfqKkh6pZU5n1zpX0pKSe7PKFAdrfJalrqOqz4akexo+kS7Mx84ikl4vG0IlDWUejG1nrAhpRRMyodQ2D8JWIuL7WRZhtVg/jJyI+D4VwBG6KiM5S7SSNjIiNQ1haQ/Ee1CBJOmvznoakiyTdmV0/QtK12fWnJI3P3tktlXSFpIcl3SZpTNbmYElLJN0LfL6o/xGSLpD0a0kPSvqbbHm3pP9WwQRJyyS9s0rbeLmkRVnN3yhx/4hs7+shSb+RNCtbvqekWyQtlrRA0rurUZ/Vr2Eyfn4h6TxJ9wCnS7pW0nFF979QdP2rkn6V1fr1atRTzxxQg3cP8KHsehfQImkUcAiwoET7vYFLI2I/YB1wQrb8KuALEfH+fu0/DayPiPcC7wU+K2lSRMwDVlEYjFcA50TEquIHStqhaKqh/2XfMttzQVGbA7JlX4uILmAKcJikKf0e0wm0RcT+EXFAti0As4EzIuJg4O+Ay8qs04avRhs/5ewYEYdGxHfLNZA0A+gAplIYUx+Q9IFBrqeheYpv8BYDB0vaAXgVuJ/CQPsQUOoznCcjoqfosRMlvQ3YKSLuzpb/ADgqu34kMKVoLvttFAbpk8AZwEPAfRFxXf8VRcTzFF7og1Fqiu8jkk6l8PqYAOwLPFh0/xPAHpK+B9wM3CapBfgA8J+SNrcbPcharPE12vgp58cVtDmSQt0PZLdbgH2AX+ZUQ91zQA1SRGyQ9BTwKQovpAeBPwX2BJaWeMirRddfB8YAAsodBFEU9kJuLXFfG7AJeIekpojY9IYHFgZ9qXehAB+NiEfK3FfcxyQKez/vjYhnJc0FmovbZMvfA/w5hXekHwG+BKwrNxdvBo0/foq8WHR9I9lslaQR/PH/XQHfiojvD6LfYcVTfNvmHgr/id9D4QX9t0BPVHjk3YhYB6yXdEi26GNFd98KfC6b9kDSPpLGShpJYVrjoxQG8pkl+n0+IjrLXCodXDtSGFzrJb2DP74z3ULSeKApIn4C/BNwUEQ8Bzwp6a+yNspCzKy/Rh4/pTwFHJxd7wZGFNX6aUljs1rbs7FlGe9BbZsFwNeAeyPiRUmvUP6dVzmfAq6U9BKFF+pmc4CJwP0qzJWtBo4DvgwsiIgFknqAX0u6OSJKvevcZhGxRNIDwMMUpvL+t0SzNuAqSZvf4Jyd/f0YcLmkfwRGUZjmWJJnfdYQGnb8lPHvwA2S/gy4jWyvMCLmZ18kui+bFn+eQoCuGYKa6oJ8ug0zM0uRp/jMzCxJDigzM0uSA8rMzJLkgDIzsyRVJaCmT58eFH6n4IsvjX7JhceML8PsUpGqBNSaNf6WpNlgeMyYvZmn+MzMLEkOKDMzS5IDyszMkuRDHZmZ1diGDRvo7e3llVdeqXUpuWpubqa9vZ1Ro0Zt0+MdUGZmNdbb28sOO+zAxIkTKTpdTV2LCNauXUtvby+TJk3apj48xWdmVmOvvPIK48aNa5hwApDEuHHj3tJeoQOqSto62pCUy6Wto63Wm2NmVdZI4bTZW90mT/FVSd+KPo6ZNyOXvm7snp9LP2Zm9cR7UGZmidl9woTcZmAksfuECQOuc8SIEXR2dm65nH/++QDccccdHHTQQXR2dnLIIYfw+OOPV3vzt/AelJlZYpavWkVva3tu/bX39Q7YZsyYMfT09Lxp+ec+9zluuOEGJk+ezGWXXca3vvUt5s6dm1ttW+M9KDMzK0sSzz33HADr16+ntbV1yNbtPSgzM+Pll1+ms7Nzy+2zzz6bmTNnMmfOHGbMmMGYMWPYcccdue+++4asJgeUmZmVneK76KKLmD9/PlOnTuWCCy7gzDPPZM6cOUNSk6f4zMyspNWrV7NkyRKmTp0KwMyZM/nlL385ZOt3QJmZWUk777wz69evZ9myZQDcfvvtTJ48ecjW7yk+M7PEdLzznRV9824w/Q2k/2dQ06dP5/zzz+eKK67ghBNOoKmpiZ133pkrr7wyt7oG4oAyM0vM/61cOeTrfP3110su7+7upru7e4irKfAUn5mZJckBZWZmSXJAmZlZkhxQZmaWJAeUmZklyQFlZmZJqiigJO0k6XpJv5W0VNL7q12Ymdlw1drekevpNlrbOwZcZ//TbTz11FNb7lu+fDktLS1ceOGFVdzqN6v0d1D/BtwSESdK2g7Yvoo1mZkNayufXsHUr9+SW38L/3n6gG3KHYsPYNasWRx11FG51VOpAQNK0o7AocApABHxGvBadcsyM7MU/OxnP2OPPfZg7NixQ77uSqb49gBWA1dJekDSHElvqlTSqZIWSVq0evXq3As1azQeM5aSzYc66uzs3HLkiBdffJFvf/vbnHPOOTWpqZKAGgkcBFweEQcCLwJf7d8oImZHRFdEdO2yyy45l2nWeDxmLCWbp/h6enqYN28eAOeccw6zZs2ipaWlJjVV8hlUL9AbEQuz29dTIqDMzKyxLFy4kOuvv56zzjqLdevW0dTURHNzM6effvqQrH/AgIqIVZJWSHpXRDwKHAE8Uv3SzMyslhYsWLDl+rnnnktLS8uQhRNU/i2+M4AfZt/gewL4VPVKMjMb3ia07VbRN+8G0189qiigIqIH6KpyLWZmBvT1Lh/ydb7wwgtbvf/cc88dmkKK+EgSZmaWJAeUmZklyQFlZmZJckCZmVmSHFBmZpYkB5SZmSXJAWVmlpi2jrZcT7fR1tFW0XrPO+889ttvP6ZMmUJnZycLFy7kkksuYa+99kISa9aseUP7u+66i87OTvbbbz8OO+yw3J+HSn+oa2ZmQ6RvRR/HzJuRW383ds8fsM29997LTTfdxP3338/o0aNZs2YNr732Gttttx1HH30006ZNe0P7devWcdppp3HLLbfQ0dHBM888k1u9mzmgzMyMlStXMn78eEaPHg3A+PHjAWhtbS3Z/kc/+hHHH388HR2FkyHuuuuuudfkKT4zM+PII49kxYoV7LPPPpx22mncfffdW22/bNkynn32WaZNm8bBBx/MNddck3tNDigzM6OlpYXFixcze/ZsdtllF2bOnMncuXPLtt+4cSOLFy/m5ptv5tZbb+Wb3/wmy5Yty7UmT/GZmRkAI0aMYNq0aUybNo0DDjiAq6++mlNOOaVk2/b2dsaPH8/YsWMZO3Yshx56KEuWLGGfffbJrR7vQZmZGY8++iiPPfbYlts9PT3svvvuZdsfe+yxLFiwgI0bN/LSSy+xcOFCJk+enGtN3oMyM0tM626tFX3zbjD9DeSFF17gjDPOYN26dYwcOZK99tqL2bNnc/HFF/Od73yHVatWMWXKFGbMmMGcOXOYPHky06dPZ8qUKTQ1NfGZz3yG/fffP7eaARQRuXYI0NXVFYsWLcq933oiKbevid7YPZ+8/p3aOtroW9GXS1+tu7Xy9PKnc+mrjimPTjxmhrelS5fmvveRijLbVtG48R7UMJPn7yvyfIdnZtafP4MyM7MkOaDMzBJQjY9bau2tbpMDysysxpqbm1m7dm1DhVREsHbtWpqbm7e5D38GZWZWY+3t7fT29rJ69epal5Kr5uZm2tvbt/nxDigzsxobNWoUkyZNqnUZyfEUn5mZJckBZWZmSXJAmZlZkhxQZmaWJAeUmZklyQFlZmZJckCZmVmSHFBmZpYkB5SZmSXJAWVmZkmqOKAkjZD0gKSbqlmQmZkZDG4P6ovA0moVYmZmVqyigJLUDvwFMKe65ZiZmRVUugf1XeAsYFO5BpJOlbRI0qJGO2R8rTWNakJSLpdU62rraMu1tnrQKGNm9wkTcnsd7D5hQq03xxIy4Ok2JB0NPBMRiyVNK9cuImYDswG6uroa56xbCdi0YRPHzJuRS183ds/PpR9It6560ShjZvmqVfS2bvs5f4q19/Xm0o81hkr2oD4I/KWkp4AfA4dLuraqVZmZ2bA3YEBFxNkR0R4RE4GTgDsj4uNVr8zMzIY1/w7KzMySNKhTvkfEXcBdVanEzMysiPegzMwsSQ4oMzNLkgPKzMyS5IAyM7MkOaDMzCxJDigzM0uSA8rMzJLkgDIzsyQ5oMzMLEkOKDMzS5IDyszMkuSAMjOzJDmgzMwsSQ4oMzNLkgPKzMySNKjzQZmZ9dc0qon2vt7c+kpRW0cbfSv6cumrdbdWnl7+dC59NToHlJm9JZs2bOKYeTNy6evG7vm59JO3vhV9Db+NKUrz7YqZmQ17DigzM0uSA8rMzJLkgDIzsyQ5oMzMLEkOKDMzS5IDyszMkuSAMjOzJDmgzMwsSQ4oMzNLkgPKzMyS5IAyM7MkOaDMzCxJDigzM0uSA8rMzJI0YEBJ2k3S/0haKulhSV8cisLMzGx4q+SEhRuBL0fE/ZJ2ABZLuj0iHqlybWZmNowNuAcVESsj4v7s+vPAUqCt2oWZmdnwNqjPoCRNBA4EFpa471RJiyQtWr16dT7VmTUwj5nqautoQ1IuF6uNSqb4AJDUAvwE+FJEPNf//oiYDcwG6OrqitwqNGtQHjPV1beij2Pmzcilrxu75+fSjw1ORXtQkkZRCKcfRsRPq1uSmZlZZd/iE/B9YGlE/Gv1SzIzM6tsD+qDwCeAwyX1ZJd89pvNzMzKGPAzqIj4BeBPCc3MbEj5SBJmZpYkB5SZmSXJAWVmZklyQJmZWZIcUGZmliQHlJmZJckBZWZmSXJAmZlZkhxQZmaWJAeUmZklyQFlZmZJckCZmVmSHFBmZpYkB5SZmSWp4lO+p2rEdiPYtGFTLn01jWrKrS8bnKZRTRTOjZlDX9s1sem1fP4dR40eyWuvbMilr7eqraONvhV9ufQ1avRINry6MZe+zKql7gNq04ZNHDMvn/Mn3tg9n97W9lz6au/rzaWf4SLvf8c8+0pF34o+v9ZtWPEUn5mZJckBZWZmSXJAmZlZkhxQZmaWJAeUmZklyQFlZmZJckCZmVmSHFBmZpYkB5SZmSXJAWVmZklyQJmZWZIcUGZmliQHlJmZJckBZWZmSXJAmZlZkhxQZmaWpIoCStJ0SY9KelzSV6tdlJmZ2YABJWkEcClwFLAvcLKkfatdmJmZDW+V7EH9CfB4RDwREa8BPwaOrW5ZZmY23Ckitt5AOhGYHhGfyW5/ApgaEaf3a3cqcGp2813Ao8B4YE3eRQ+xet8G119dayJi+rY8sMyYgfS3eSD1Xj/U/zakXn9F42ZkBR2pxLI3pVpEzAZmv+GB0qKI6KpgHcmq921w/ekqNWag/re53uuH+t+Geq9/s0qm+HqB3YputwN91SnHzMysoJKA+jWwt6RJkrYDTgJ+Xt2yzMxsuBtwii8iNko6HbgVGAFcGREPV9j/m6Yv6lC9b4Prrz/1vs31Xj/U/zbUe/1ABV+SMDMzqwUfScLMzJLkgDIzsyTlGlCSZkl6WNJDkq6T1CxprqQnJfVkl84815knSV/Man9Y0peyZW+XdLukx7K/O9e6znLK1H+upKeLnv8Zta6zmKQrJT0j6aGiZSWfcxVcnB1y60FJB9Wu8nzU+5gBj5uhNpzGTG4BJakN+ALQFRH7U/hCxUnZ3V+JiM7s0pPXOvMkaX/gsxSOnPEe4GhJewNfBe6IiL2BO7LbydlK/QAXFT3/82tWZGlzgf4/2Cv3nB8F7J1dTgUuH6Iaq6Lexwx43NTIXIbJmMl7im8kMEbSSGB76uv3UpOB+yLipYjYCNwNdFM4rNPVWZurgeNqVN9AytWftIi4B/hDv8XlnvNjgWui4D5gJ0kThqbSqqnnMQMeN0NuOI2Z3AIqIp4GLgSWAyuB9RFxW3b3ednu5UWSRue1zpw9BBwqaZyk7YEZFH6g/I6IWAmQ/d21hjVuTbn6AU7Pnv8rU55qKVLuOW8DVhS1682W1aUGGDPgcZOKhhwzeU7x7UwhrScBrcBYSR8HzgbeDbwXeDvw93mtM08RsRT4NnA7cAuwBNhY06IGYSv1Xw7sCXRS+E/wX2pVYw4qOuxWvaj3MQMeN3WgrsdMnlN8HwaejIjVEbEB+CnwgYhYme1evgpcRWGuN0kR8f2IOCgiDqWwC/0Y8PvNu8TZ32dqWePWlKo/In4fEa9HxCbgChJ+/ouUe84b7bBbdT9mwOMmEQ05ZvIMqOXA+yRtL0nAEcDSoidNFOZFH9pKHzUladfsbwdwPHAdhcM6fTJr8knghtpUN7BS9febb+4m4ee/SLnn/OfAX2ffTHofhSmxlbUoMCd1P2bA4yYRjTlmIiK3C/AN4LcU/jF/AIwG7gR+ky27FmjJc505178AeITCbv4R2bJxFL4V81j29+21rnOQ9f8ge/4fpPBinVDrOvvVfB2FKZQNFN7tfbrcc05huuJS4HfZNnXVuv4ctr+ux8xWXnceN9Wrd9iMGR/qyMzMkuQjSZiZWZIcUGZmliQHlJmZJckBZWZmSXJAmZlZkhxQCZLULSkkvTu7PVHSy5IekLRU0q8kfbKo/SmSLsmuFx+F+TFJP5W0b1HbuyQ9WnSU5uuHfgvN8udx03gGPOW71cTJwC8oHNn63GzZ7yLiQABJewA/ldQUEVeVePxFEXFh1nYmcKekAyJidXb/xyJiUVW3wGzoedw0GO9BJUZSC/BBCj++O6lUm4h4AjiTwqkatioi/gO4DfhojmWaJcXjpjE5oNJzHHBLRCwD/qDyJxi7n8IBRSvRv+0Pi6YqLngLtZqlwuOmAXmKLz0nA9/Nrv84u31piXaljlJcTv+2nqqwRuNx04AcUAmRNA44HNhfUlA4w2oAl5VofiCwtMKuDwQ8sKwhedw0Lk/xpeVECme/3D0iJkbEbsCTFA6Rv4WkiRROdPe9gTqUdAJwJIUDTJo1Io+bBuU9qLScDJzfb9lPgH8A9pT0ANAMPA98r+ibSCOBV4seMys78d1YCkfEPrzom0hQmEt/Obu+JiI+nPN2mA0lj5sG5aOZNwBJF1E4yVqpKQ0zK8HjJn0OqDon6b+A7YDjI2J9resxqwceN/XBAWVmZknylyTMzCxJDigzM0uSA8rMzJLkgDIzsyQ5oMzMLEn/D5atggtp71QbAAAAAElFTkSuQmCC\n",
                        "text/plain": "<Figure size 432x216 with 2 Axes>"
                    },
                    "metadata": {
                        "needs_background": "light"
                    },
                    "output_type": "display_data"
                }
            ],
            "source": "bins = np.linspace(df1.ADJDE.min(), df1.ADJDE.max(), 10)\ng = sns.FacetGrid(df1, col=\"windex\", hue=\"POSTSEASON\", palette=\"Set1\", col_wrap=2)\ng.map(plt.hist, 'ADJDE', bins=bins, ec=\"k\")\ng.axes[-1].legend()\nplt.show()\n"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "We see that this data point doesn't impact the ability of a team to get into the Final Four. "
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "## Convert Categorical features to numerical values"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "Lets look at the postseason:"
        },
        {
            "cell_type": "code",
            "execution_count": 12,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [
                {
                    "data": {
                        "text/plain": "windex  POSTSEASON\nFalse   S16           0.605263\n        E8            0.263158\n        F4            0.131579\nTrue    S16           0.500000\n        E8            0.333333\n        F4            0.166667\nName: POSTSEASON, dtype: float64"
                    },
                    "execution_count": 12,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "df1.groupby(['windex'])['POSTSEASON'].value_counts(normalize=True)"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "13% of teams with 6 or less wins above bubble make it into the final four while 17% of teams with 7 or more do.\n"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "Lets convert wins above bubble (winindex) under 7 to 0 and over 7 to 1:\n"
        },
        {
            "cell_type": "code",
            "execution_count": 13,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [
                {
                    "data": {
                        "text/html": "<div>\n<style scoped>\n    .dataframe tbody tr th:only-of-type {\n        vertical-align: middle;\n    }\n\n    .dataframe tbody tr th {\n        vertical-align: top;\n    }\n\n    .dataframe thead th {\n        text-align: right;\n    }\n</style>\n<table border=\"1\" class=\"dataframe\">\n  <thead>\n    <tr style=\"text-align: right;\">\n      <th></th>\n      <th>TEAM</th>\n      <th>CONF</th>\n      <th>G</th>\n      <th>W</th>\n      <th>ADJOE</th>\n      <th>ADJDE</th>\n      <th>BARTHAG</th>\n      <th>EFG_O</th>\n      <th>EFG_D</th>\n      <th>TOR</th>\n      <th>...</th>\n      <th>2P_O</th>\n      <th>2P_D</th>\n      <th>3P_O</th>\n      <th>3P_D</th>\n      <th>ADJ_T</th>\n      <th>WAB</th>\n      <th>POSTSEASON</th>\n      <th>SEED</th>\n      <th>YEAR</th>\n      <th>windex</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>0</th>\n      <td>North Carolina</td>\n      <td>ACC</td>\n      <td>40</td>\n      <td>33</td>\n      <td>123.3</td>\n      <td>94.9</td>\n      <td>0.9531</td>\n      <td>52.6</td>\n      <td>48.1</td>\n      <td>15.4</td>\n      <td>...</td>\n      <td>53.9</td>\n      <td>44.6</td>\n      <td>32.7</td>\n      <td>36.2</td>\n      <td>71.7</td>\n      <td>8.6</td>\n      <td>2ND</td>\n      <td>1.0</td>\n      <td>2016</td>\n      <td>1</td>\n    </tr>\n    <tr>\n      <th>1</th>\n      <td>Villanova</td>\n      <td>BE</td>\n      <td>40</td>\n      <td>35</td>\n      <td>123.1</td>\n      <td>90.9</td>\n      <td>0.9703</td>\n      <td>56.1</td>\n      <td>46.7</td>\n      <td>16.3</td>\n      <td>...</td>\n      <td>57.4</td>\n      <td>44.1</td>\n      <td>36.2</td>\n      <td>33.9</td>\n      <td>66.7</td>\n      <td>8.9</td>\n      <td>Champions</td>\n      <td>2.0</td>\n      <td>2016</td>\n      <td>1</td>\n    </tr>\n    <tr>\n      <th>2</th>\n      <td>Notre Dame</td>\n      <td>ACC</td>\n      <td>36</td>\n      <td>24</td>\n      <td>118.3</td>\n      <td>103.3</td>\n      <td>0.8269</td>\n      <td>54.0</td>\n      <td>49.5</td>\n      <td>15.3</td>\n      <td>...</td>\n      <td>52.9</td>\n      <td>46.5</td>\n      <td>37.4</td>\n      <td>36.9</td>\n      <td>65.5</td>\n      <td>2.3</td>\n      <td>E8</td>\n      <td>6.0</td>\n      <td>2016</td>\n      <td>0</td>\n    </tr>\n    <tr>\n      <th>3</th>\n      <td>Virginia</td>\n      <td>ACC</td>\n      <td>37</td>\n      <td>29</td>\n      <td>119.9</td>\n      <td>91.0</td>\n      <td>0.9600</td>\n      <td>54.8</td>\n      <td>48.4</td>\n      <td>15.1</td>\n      <td>...</td>\n      <td>52.6</td>\n      <td>46.3</td>\n      <td>40.3</td>\n      <td>34.7</td>\n      <td>61.9</td>\n      <td>8.6</td>\n      <td>E8</td>\n      <td>1.0</td>\n      <td>2016</td>\n      <td>1</td>\n    </tr>\n    <tr>\n      <th>4</th>\n      <td>Kansas</td>\n      <td>B12</td>\n      <td>37</td>\n      <td>32</td>\n      <td>120.9</td>\n      <td>90.4</td>\n      <td>0.9662</td>\n      <td>55.7</td>\n      <td>45.1</td>\n      <td>17.8</td>\n      <td>...</td>\n      <td>52.7</td>\n      <td>43.4</td>\n      <td>41.3</td>\n      <td>32.5</td>\n      <td>70.1</td>\n      <td>11.6</td>\n      <td>E8</td>\n      <td>1.0</td>\n      <td>2016</td>\n      <td>1</td>\n    </tr>\n  </tbody>\n</table>\n<p>5 rows \u00d7 25 columns</p>\n</div>",
                        "text/plain": "             TEAM CONF   G   W  ADJOE  ADJDE  BARTHAG  EFG_O  EFG_D   TOR  \\\n0  North Carolina  ACC  40  33  123.3   94.9   0.9531   52.6   48.1  15.4   \n1       Villanova   BE  40  35  123.1   90.9   0.9703   56.1   46.7  16.3   \n2      Notre Dame  ACC  36  24  118.3  103.3   0.8269   54.0   49.5  15.3   \n3        Virginia  ACC  37  29  119.9   91.0   0.9600   54.8   48.4  15.1   \n4          Kansas  B12  37  32  120.9   90.4   0.9662   55.7   45.1  17.8   \n\n   ...  2P_O  2P_D  3P_O  3P_D  ADJ_T   WAB  POSTSEASON  SEED  YEAR  windex  \n0  ...  53.9  44.6  32.7  36.2   71.7   8.6         2ND   1.0  2016       1  \n1  ...  57.4  44.1  36.2  33.9   66.7   8.9   Champions   2.0  2016       1  \n2  ...  52.9  46.5  37.4  36.9   65.5   2.3          E8   6.0  2016       0  \n3  ...  52.6  46.3  40.3  34.7   61.9   8.6          E8   1.0  2016       1  \n4  ...  52.7  43.4  41.3  32.5   70.1  11.6          E8   1.0  2016       1  \n\n[5 rows x 25 columns]"
                    },
                    "execution_count": 13,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "df['windex'].replace(to_replace=['False','True'], value=[0,1],inplace=True)\ndf.head()"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "## One Hot Encoding  \n#### How about seed?"
        },
        {
            "cell_type": "code",
            "execution_count": 14,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [
                {
                    "data": {
                        "text/plain": "SEED  POSTSEASON\n1.0   E8            0.750000\n      F4            0.125000\n      S16           0.125000\n2.0   S16           0.444444\n      E8            0.333333\n      F4            0.222222\n3.0   S16           0.700000\n      E8            0.200000\n      F4            0.100000\n4.0   S16           0.875000\n      E8            0.125000\n5.0   S16           0.833333\n      F4            0.166667\n6.0   E8            1.000000\n7.0   S16           0.800000\n      F4            0.200000\n8.0   S16           1.000000\n9.0   E8            1.000000\n10.0  F4            1.000000\n11.0  S16           0.500000\n      E8            0.250000\n      F4            0.250000\n12.0  S16           1.000000\nName: POSTSEASON, dtype: float64"
                    },
                    "execution_count": 14,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "df1.groupby(['SEED'])['POSTSEASON'].value_counts(normalize=True)"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "#### Feature before One Hot Encoding"
        },
        {
            "cell_type": "code",
            "execution_count": 15,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [
                {
                    "data": {
                        "text/html": "<div>\n<style scoped>\n    .dataframe tbody tr th:only-of-type {\n        vertical-align: middle;\n    }\n\n    .dataframe tbody tr th {\n        vertical-align: top;\n    }\n\n    .dataframe thead th {\n        text-align: right;\n    }\n</style>\n<table border=\"1\" class=\"dataframe\">\n  <thead>\n    <tr style=\"text-align: right;\">\n      <th></th>\n      <th>ADJOE</th>\n      <th>ADJDE</th>\n      <th>BARTHAG</th>\n      <th>EFG_O</th>\n      <th>EFG_D</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>2</th>\n      <td>118.3</td>\n      <td>103.3</td>\n      <td>0.8269</td>\n      <td>54.0</td>\n      <td>49.5</td>\n    </tr>\n    <tr>\n      <th>3</th>\n      <td>119.9</td>\n      <td>91.0</td>\n      <td>0.9600</td>\n      <td>54.8</td>\n      <td>48.4</td>\n    </tr>\n    <tr>\n      <th>4</th>\n      <td>120.9</td>\n      <td>90.4</td>\n      <td>0.9662</td>\n      <td>55.7</td>\n      <td>45.1</td>\n    </tr>\n    <tr>\n      <th>5</th>\n      <td>118.4</td>\n      <td>96.2</td>\n      <td>0.9163</td>\n      <td>52.3</td>\n      <td>48.9</td>\n    </tr>\n    <tr>\n      <th>6</th>\n      <td>111.9</td>\n      <td>93.6</td>\n      <td>0.8857</td>\n      <td>50.0</td>\n      <td>47.3</td>\n    </tr>\n  </tbody>\n</table>\n</div>",
                        "text/plain": "   ADJOE  ADJDE  BARTHAG  EFG_O  EFG_D\n2  118.3  103.3   0.8269   54.0   49.5\n3  119.9   91.0   0.9600   54.8   48.4\n4  120.9   90.4   0.9662   55.7   45.1\n5  118.4   96.2   0.9163   52.3   48.9\n6  111.9   93.6   0.8857   50.0   47.3"
                    },
                    "execution_count": 15,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "df1[['ADJOE','ADJDE','BARTHAG','EFG_O','EFG_D']].head()"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "#### Use one hot encoding technique to conver categorical varables to binary variables and append them to the feature Data Frame "
        },
        {
            "cell_type": "code",
            "execution_count": 16,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [
                {
                    "data": {
                        "text/html": "<div>\n<style scoped>\n    .dataframe tbody tr th:only-of-type {\n        vertical-align: middle;\n    }\n\n    .dataframe tbody tr th {\n        vertical-align: top;\n    }\n\n    .dataframe thead th {\n        text-align: right;\n    }\n</style>\n<table border=\"1\" class=\"dataframe\">\n  <thead>\n    <tr style=\"text-align: right;\">\n      <th></th>\n      <th>ADJOE</th>\n      <th>ADJDE</th>\n      <th>BARTHAG</th>\n      <th>EFG_O</th>\n      <th>EFG_D</th>\n      <th>E8</th>\n      <th>F4</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>2</th>\n      <td>118.3</td>\n      <td>103.3</td>\n      <td>0.8269</td>\n      <td>54.0</td>\n      <td>49.5</td>\n      <td>1</td>\n      <td>0</td>\n    </tr>\n    <tr>\n      <th>3</th>\n      <td>119.9</td>\n      <td>91.0</td>\n      <td>0.9600</td>\n      <td>54.8</td>\n      <td>48.4</td>\n      <td>1</td>\n      <td>0</td>\n    </tr>\n    <tr>\n      <th>4</th>\n      <td>120.9</td>\n      <td>90.4</td>\n      <td>0.9662</td>\n      <td>55.7</td>\n      <td>45.1</td>\n      <td>1</td>\n      <td>0</td>\n    </tr>\n    <tr>\n      <th>5</th>\n      <td>118.4</td>\n      <td>96.2</td>\n      <td>0.9163</td>\n      <td>52.3</td>\n      <td>48.9</td>\n      <td>1</td>\n      <td>0</td>\n    </tr>\n    <tr>\n      <th>6</th>\n      <td>111.9</td>\n      <td>93.6</td>\n      <td>0.8857</td>\n      <td>50.0</td>\n      <td>47.3</td>\n      <td>0</td>\n      <td>1</td>\n    </tr>\n  </tbody>\n</table>\n</div>",
                        "text/plain": "   ADJOE  ADJDE  BARTHAG  EFG_O  EFG_D  E8  F4\n2  118.3  103.3   0.8269   54.0   49.5   1   0\n3  119.9   91.0   0.9600   54.8   48.4   1   0\n4  120.9   90.4   0.9662   55.7   45.1   1   0\n5  118.4   96.2   0.9163   52.3   48.9   1   0\n6  111.9   93.6   0.8857   50.0   47.3   0   1"
                    },
                    "execution_count": 16,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "Feature = df1[['ADJOE','ADJDE','BARTHAG','EFG_O','EFG_D']]\nFeature = pd.concat([Feature,pd.get_dummies(df1['POSTSEASON'])], axis=1)\nFeature.drop(['S16'], axis = 1,inplace=True)\nFeature.head()\n"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "### Feature selection"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "Lets defind feature sets, X:"
        },
        {
            "cell_type": "code",
            "execution_count": 17,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [
                {
                    "data": {
                        "text/html": "<div>\n<style scoped>\n    .dataframe tbody tr th:only-of-type {\n        vertical-align: middle;\n    }\n\n    .dataframe tbody tr th {\n        vertical-align: top;\n    }\n\n    .dataframe thead th {\n        text-align: right;\n    }\n</style>\n<table border=\"1\" class=\"dataframe\">\n  <thead>\n    <tr style=\"text-align: right;\">\n      <th></th>\n      <th>ADJOE</th>\n      <th>ADJDE</th>\n      <th>BARTHAG</th>\n      <th>EFG_O</th>\n      <th>EFG_D</th>\n      <th>E8</th>\n      <th>F4</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>2</th>\n      <td>118.3</td>\n      <td>103.3</td>\n      <td>0.8269</td>\n      <td>54.0</td>\n      <td>49.5</td>\n      <td>1</td>\n      <td>0</td>\n    </tr>\n    <tr>\n      <th>3</th>\n      <td>119.9</td>\n      <td>91.0</td>\n      <td>0.9600</td>\n      <td>54.8</td>\n      <td>48.4</td>\n      <td>1</td>\n      <td>0</td>\n    </tr>\n    <tr>\n      <th>4</th>\n      <td>120.9</td>\n      <td>90.4</td>\n      <td>0.9662</td>\n      <td>55.7</td>\n      <td>45.1</td>\n      <td>1</td>\n      <td>0</td>\n    </tr>\n    <tr>\n      <th>5</th>\n      <td>118.4</td>\n      <td>96.2</td>\n      <td>0.9163</td>\n      <td>52.3</td>\n      <td>48.9</td>\n      <td>1</td>\n      <td>0</td>\n    </tr>\n    <tr>\n      <th>6</th>\n      <td>111.9</td>\n      <td>93.6</td>\n      <td>0.8857</td>\n      <td>50.0</td>\n      <td>47.3</td>\n      <td>0</td>\n      <td>1</td>\n    </tr>\n  </tbody>\n</table>\n</div>",
                        "text/plain": "   ADJOE  ADJDE  BARTHAG  EFG_O  EFG_D  E8  F4\n2  118.3  103.3   0.8269   54.0   49.5   1   0\n3  119.9   91.0   0.9600   54.8   48.4   1   0\n4  120.9   90.4   0.9662   55.7   45.1   1   0\n5  118.4   96.2   0.9163   52.3   48.9   1   0\n6  111.9   93.6   0.8857   50.0   47.3   0   1"
                    },
                    "execution_count": 17,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "X = Feature\nX[0:5]"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "What are our lables? Round where the given team was eliminated or where their season ended (R68 = First Four, R64 = Round of 64, R32 = Round of 32, S16 = Sweet Sixteen, E8 = Elite Eight, F4 = Final Four, 2ND = Runner-up, Champion = Winner of the NCAA March Madness Tournament for that given year)|"
        },
        {
            "cell_type": "code",
            "execution_count": 18,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [
                {
                    "data": {
                        "text/plain": "array(['E8', 'E8', 'E8', 'E8', 'F4'], dtype=object)"
                    },
                    "execution_count": 18,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "y = df1['POSTSEASON'].values\ny[0:5]"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "## Normalize Data "
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "Data Standardization give data zero mean and unit variance (technically should be done after train test split )"
        },
        {
            "cell_type": "code",
            "execution_count": 19,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [
                {
                    "name": "stderr",
                    "output_type": "stream",
                    "text": "/opt/conda/envs/Python36/lib/python3.6/site-packages/sklearn/preprocessing/data.py:645: DataConversionWarning: Data with input dtype uint8, float64 were all converted to float64 by StandardScaler.\n  return self.partial_fit(X, y)\n/opt/conda/envs/Python36/lib/python3.6/site-packages/ipykernel/__main__.py:1: DataConversionWarning: Data with input dtype uint8, float64 were all converted to float64 by StandardScaler.\n  if __name__ == '__main__':\n"
                },
                {
                    "data": {
                        "text/plain": "array([[ 0.28034482,  2.74329908, -2.45717765,  0.10027963,  0.94171924,\n         1.58113883, -0.40824829],\n       [ 0.64758014, -0.90102957,  1.127076  ,  0.39390887,  0.38123706,\n         1.58113883, -0.40824829],\n       [ 0.87710222, -1.0788017 ,  1.29403598,  0.72424177, -1.30020946,\n         1.58113883, -0.40824829],\n       [ 0.30329703,  0.63966222, -0.04972253, -0.52368251,  0.63600169,\n         1.58113883, -0.40824829],\n       [-1.18859646, -0.13068368, -0.87375079, -1.36786658, -0.17924511,\n        -0.63245553,  2.44948974]])"
                    },
                    "execution_count": 19,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "X= preprocessing.StandardScaler().fit(X).transform(X)\nX[0:5]"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "## Training and Validation "
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "Split the data into Training and Validation data."
        },
        {
            "cell_type": "code",
            "execution_count": 20,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [
                {
                    "name": "stdout",
                    "output_type": "stream",
                    "text": "Train set: (44, 7) (44,)\nValidation set: (12, 7) (12,)\n"
                }
            ],
            "source": "# We split the X into train and test to find the best k\nfrom sklearn.model_selection import train_test_split\nX_train, X_val, y_train, y_val = train_test_split(X, y, test_size=0.2, random_state=4)\nprint ('Train set:', X_train.shape,  y_train.shape)\nprint ('Validation set:', X_val.shape,  y_val.shape)"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "# Classification "
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "Now, it is your turn, use the training set to build an accurate model. Then use the validation set  to report the accuracy of the model\nYou should use the following algorithm:\n- K Nearest Neighbor(KNN)\n- Decision Tree\n- Support Vector Machine\n- Logistic Regression\n\n"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "# K Nearest Neighbor(KNN)\n\n<b>Question  1 </b> Build a KNN model using a value of k equals three, find the accuracy on the validation data (X_val and y_val)"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "You can use <code> accuracy_score</cdoe>"
        },
        {
            "cell_type": "code",
            "execution_count": 55,
            "metadata": {},
            "outputs": [
                {
                    "name": "stdout",
                    "output_type": "stream",
                    "text": "KNeighborsClassifier(algorithm='auto', leaf_size=30, metric='minkowski',\n           metric_params=None, n_jobs=None, n_neighbors=3, p=2,\n           weights='uniform') \n\nSample of five results from prediction: ['F4' 'S16' 'S16' 'S16' 'S16'] \n\nValidation set accuracy: 1.0\n"
                }
            ],
            "source": "from sklearn.metrics import accuracy_score\nfrom sklearn.neighbors import KNeighborsClassifier\nfrom sklearn import metrics\n\n#set the k value for the model\nk = 3\n#training the model and predict\nneigh = KNeighborsClassifier(n_neighbors=k).fit(X_train,y_train)\nyhat = neigh.predict(X_val)\n\nprint(neigh,\"\\n\")\nprint(\"Sample of five results from prediction:\",yhat[0:5],'\\n')\nprint(\"Validation set accuracy:\",metrics.accuracy_score(y_val,yhat))"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "<b>Question  2</b> Determine the accuracy for the first 15 values of k the on the validation data:"
        },
        {
            "cell_type": "code",
            "execution_count": 23,
            "metadata": {},
            "outputs": [
                {
                    "data": {
                        "text/plain": "array([1.        , 1.        , 1.        , 1.        , 1.        ,\n       1.        , 0.91666667, 0.91666667, 0.83333333, 0.83333333,\n       0.83333333, 0.83333333, 0.83333333, 0.83333333])"
                    },
                    "execution_count": 23,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "from sklearn import metrics\n\nKs = 15\nmean_acc =  np.zeros((Ks-1))\nstd_acc = np.zeros((Ks-1))\nConfusion_matrix = []\nfor n in range(1,Ks):\n    neigh = KNeighborsClassifier(n_neighbors=n).fit(X_train,y_train)\n    yhat = neigh.predict(X_val)\n    mean_acc[n-1] = metrics.accuracy_score(y_val,yhat)\n    std_acc[n-1] = np.std(yhat==y_val)/np.sqrt(yhat.shape[0])\n    \nmean_acc\n\n        "
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "# Decision Tree"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "The following lines of code fit a <code>DecisionTreeClassifier</code>:"
        },
        {
            "cell_type": "code",
            "execution_count": 24,
            "metadata": {},
            "outputs": [],
            "source": "from sklearn.tree import DecisionTreeClassifier"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "<b>Question  3</b> Determine the minumum   value for the parameter <code>max_depth</code> that improves results "
        },
        {
            "cell_type": "code",
            "execution_count": 25,
            "metadata": {},
            "outputs": [
                {
                    "name": "stdout",
                    "output_type": "stream",
                    "text": "The minumun value of max_depth that improves the result is: 2\n"
                }
            ],
            "source": "md = 10\nmean_acc = np.zeros((md-1))\nstd_acc = np.zeros((md-1))\n\nfor n in range(1,md):\n    Dtree = DecisionTreeClassifier(criterion='entropy',max_depth=n).fit(X_train,y_train)\n    yhat_dt = Dtree.predict(X_val)\n    mean_acc[n-1]=metrics.accuracy_score(y_val,yhat_dt)\n    std_acc[n-1]=np.std(yhat_dt==y_val)/np.sqrt(yhat_dt.shape[0])\n    \nprint(\"The minumun value of max_depth that improves the result is:\",mean_acc.argmax()+1)"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "# Support Vector Machine"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "<b>Question  4</b>Train the following linear  support  vector machine model and determine the accuracy on the validation data "
        },
        {
            "cell_type": "code",
            "execution_count": 30,
            "metadata": {},
            "outputs": [
                {
                    "name": "stdout",
                    "output_type": "stream",
                    "text": "Sample of five results from prediction: ['F4' 'S16' 'S16' 'S16' 'S16']\n"
                },
                {
                    "name": "stderr",
                    "output_type": "stream",
                    "text": "/opt/conda/envs/Python36/lib/python3.6/site-packages/sklearn/svm/base.py:196: FutureWarning: The default value of gamma will change from 'auto' to 'scale' in version 0.22 to account better for unscaled features. Set gamma explicitly to 'auto' or 'scale' to avoid this warning.\n  \"avoid this warning.\", FutureWarning)\n"
                }
            ],
            "source": "from sklearn import svm\n\n#fitting the model and predict\nclf = svm.SVC(kernel='rbf')\nclf.fit(X_train,y_train)\nyhat_svm = clf.predict(X_val)\n\nprint(\"Sample of five results from prediction:\",yhat_svm[:5])\n"
        },
        {
            "cell_type": "code",
            "execution_count": 31,
            "metadata": {},
            "outputs": [
                {
                    "name": "stdout",
                    "output_type": "stream",
                    "text": "The F1-score is: 1.0 \n\nThe Jaccard Score is: 1.0\n"
                }
            ],
            "source": "#Evaluation\nfrom sklearn.metrics import f1_score, jaccard_similarity_score\n\nprint(\"The F1-score is:\",f1_score(y_val,yhat_svm,average='weighted'),\"\\n\")\n\nprint(\"The Jaccard Score is:\",jaccard_similarity_score(y_val,yhat_svm))\n"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "# Logistic Regression"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "<b>Question 5</b> Train a logistic regression model and determine the accuracy of the validation data (set C=0.01)"
        },
        {
            "cell_type": "code",
            "execution_count": 32,
            "metadata": {},
            "outputs": [
                {
                    "name": "stdout",
                    "output_type": "stream",
                    "text": "Sample of five results from prediction: ['F4' 'S16' 'S16' 'S16' 'S16']\nSample of five results from probability prediction: [[0.28989109 0.3816511  0.32845781]\n [0.31791461 0.31327122 0.36881417]\n [0.31464004 0.31447207 0.37088789]\n [0.32345584 0.31780731 0.35873686]\n [0.31498614 0.31654477 0.36846909]]\n"
                },
                {
                    "name": "stderr",
                    "output_type": "stream",
                    "text": "/opt/conda/envs/Python36/lib/python3.6/site-packages/sklearn/linear_model/logistic.py:460: FutureWarning: Default multi_class will be changed to 'auto' in 0.22. Specify the multi_class option to silence this warning.\n  \"this warning.\", FutureWarning)\n"
                }
            ],
            "source": "from sklearn.linear_model import LogisticRegression\n\n#fitting the model and forecast\nLR = LogisticRegression(C=0.01,solver='liblinear').fit(X_train,y_train)\nyhat_LR = LR.predict(X_val)\nprint(\"Sample of five results from prediction:\",yhat_LR[:5])\n\n#forecast probability of being in S16, E8, and F4\nyhat_prob = LR.predict_proba(X_val)\nprint(\"Sample of five results from probability prediction:\",yhat_prob[:5])"
        },
        {
            "cell_type": "code",
            "execution_count": 37,
            "metadata": {},
            "outputs": [
                {
                    "name": "stdout",
                    "output_type": "stream",
                    "text": "The Jaccard Score is: 1.0 \n\nThe LogLoss is:0.98\n"
                }
            ],
            "source": "from sklearn.metrics import log_loss\n\nprint(\"The Jaccard Score is:\",jaccard_similarity_score(y_val,yhat_LR),\"\\n\")\nprint(\"The LogLoss is:%.2f\"%log_loss(y_val, yhat_prob))"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "# Model Evaluation using Test set"
        },
        {
            "cell_type": "code",
            "execution_count": 38,
            "metadata": {},
            "outputs": [],
            "source": "from sklearn.metrics import f1_score\nfrom sklearn.metrics import log_loss"
        },
        {
            "cell_type": "code",
            "execution_count": 39,
            "metadata": {},
            "outputs": [],
            "source": "def jaccard_index(predictions, true):\n    if (len(predictions) == len(true)):\n        intersect = 0;\n        for x,y in zip(predictions, true):\n            if (x == y):\n                intersect += 1\n        return intersect / (len(predictions) + len(true) - intersect)\n    else:\n        return -1"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "<b>Question  5</b> Calculate the  F1 score and Jaccard Similarity score for each model from above. Use the Hyperparameter that performed best on the validation data."
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "### Load Test set for evaluation "
        },
        {
            "cell_type": "code",
            "execution_count": 40,
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "outputs": [
                {
                    "data": {
                        "text/html": "<div>\n<style scoped>\n    .dataframe tbody tr th:only-of-type {\n        vertical-align: middle;\n    }\n\n    .dataframe tbody tr th {\n        vertical-align: top;\n    }\n\n    .dataframe thead th {\n        text-align: right;\n    }\n</style>\n<table border=\"1\" class=\"dataframe\">\n  <thead>\n    <tr style=\"text-align: right;\">\n      <th></th>\n      <th>TEAM</th>\n      <th>CONF</th>\n      <th>G</th>\n      <th>W</th>\n      <th>ADJOE</th>\n      <th>ADJDE</th>\n      <th>BARTHAG</th>\n      <th>EFG_O</th>\n      <th>EFG_D</th>\n      <th>TOR</th>\n      <th>...</th>\n      <th>FTRD</th>\n      <th>2P_O</th>\n      <th>2P_D</th>\n      <th>3P_O</th>\n      <th>3P_D</th>\n      <th>ADJ_T</th>\n      <th>WAB</th>\n      <th>POSTSEASON</th>\n      <th>SEED</th>\n      <th>YEAR</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>0</th>\n      <td>North Carolina</td>\n      <td>ACC</td>\n      <td>40</td>\n      <td>33</td>\n      <td>123.3</td>\n      <td>94.9</td>\n      <td>0.9531</td>\n      <td>52.6</td>\n      <td>48.1</td>\n      <td>15.4</td>\n      <td>...</td>\n      <td>30.4</td>\n      <td>53.9</td>\n      <td>44.6</td>\n      <td>32.7</td>\n      <td>36.2</td>\n      <td>71.7</td>\n      <td>8.6</td>\n      <td>2ND</td>\n      <td>1.0</td>\n      <td>2016</td>\n    </tr>\n    <tr>\n      <th>1</th>\n      <td>Villanova</td>\n      <td>BE</td>\n      <td>40</td>\n      <td>35</td>\n      <td>123.1</td>\n      <td>90.9</td>\n      <td>0.9703</td>\n      <td>56.1</td>\n      <td>46.7</td>\n      <td>16.3</td>\n      <td>...</td>\n      <td>30.0</td>\n      <td>57.4</td>\n      <td>44.1</td>\n      <td>36.2</td>\n      <td>33.9</td>\n      <td>66.7</td>\n      <td>8.9</td>\n      <td>Champions</td>\n      <td>2.0</td>\n      <td>2016</td>\n    </tr>\n    <tr>\n      <th>2</th>\n      <td>Notre Dame</td>\n      <td>ACC</td>\n      <td>36</td>\n      <td>24</td>\n      <td>118.3</td>\n      <td>103.3</td>\n      <td>0.8269</td>\n      <td>54.0</td>\n      <td>49.5</td>\n      <td>15.3</td>\n      <td>...</td>\n      <td>26.0</td>\n      <td>52.9</td>\n      <td>46.5</td>\n      <td>37.4</td>\n      <td>36.9</td>\n      <td>65.5</td>\n      <td>2.3</td>\n      <td>E8</td>\n      <td>6.0</td>\n      <td>2016</td>\n    </tr>\n    <tr>\n      <th>3</th>\n      <td>Virginia</td>\n      <td>ACC</td>\n      <td>37</td>\n      <td>29</td>\n      <td>119.9</td>\n      <td>91.0</td>\n      <td>0.9600</td>\n      <td>54.8</td>\n      <td>48.4</td>\n      <td>15.1</td>\n      <td>...</td>\n      <td>33.4</td>\n      <td>52.6</td>\n      <td>46.3</td>\n      <td>40.3</td>\n      <td>34.7</td>\n      <td>61.9</td>\n      <td>8.6</td>\n      <td>E8</td>\n      <td>1.0</td>\n      <td>2016</td>\n    </tr>\n    <tr>\n      <th>4</th>\n      <td>Kansas</td>\n      <td>B12</td>\n      <td>37</td>\n      <td>32</td>\n      <td>120.9</td>\n      <td>90.4</td>\n      <td>0.9662</td>\n      <td>55.7</td>\n      <td>45.1</td>\n      <td>17.8</td>\n      <td>...</td>\n      <td>37.3</td>\n      <td>52.7</td>\n      <td>43.4</td>\n      <td>41.3</td>\n      <td>32.5</td>\n      <td>70.1</td>\n      <td>11.6</td>\n      <td>E8</td>\n      <td>1.0</td>\n      <td>2016</td>\n    </tr>\n  </tbody>\n</table>\n<p>5 rows \u00d7 24 columns</p>\n</div>",
                        "text/plain": "             TEAM CONF   G   W  ADJOE  ADJDE  BARTHAG  EFG_O  EFG_D   TOR  \\\n0  North Carolina  ACC  40  33  123.3   94.9   0.9531   52.6   48.1  15.4   \n1       Villanova   BE  40  35  123.1   90.9   0.9703   56.1   46.7  16.3   \n2      Notre Dame  ACC  36  24  118.3  103.3   0.8269   54.0   49.5  15.3   \n3        Virginia  ACC  37  29  119.9   91.0   0.9600   54.8   48.4  15.1   \n4          Kansas  B12  37  32  120.9   90.4   0.9662   55.7   45.1  17.8   \n\n   ...  FTRD  2P_O  2P_D  3P_O  3P_D  ADJ_T   WAB  POSTSEASON  SEED  YEAR  \n0  ...  30.4  53.9  44.6  32.7  36.2   71.7   8.6         2ND   1.0  2016  \n1  ...  30.0  57.4  44.1  36.2  33.9   66.7   8.9   Champions   2.0  2016  \n2  ...  26.0  52.9  46.5  37.4  36.9   65.5   2.3          E8   6.0  2016  \n3  ...  33.4  52.6  46.3  40.3  34.7   61.9   8.6          E8   1.0  2016  \n4  ...  37.3  52.7  43.4  41.3  32.5   70.1  11.6          E8   1.0  2016  \n\n[5 rows x 24 columns]"
                    },
                    "execution_count": 40,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "test_df = pd.read_csv('https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/ML0120ENv3/Dataset/ML0101EN_EDX_skill_up/basketball_train.csv',error_bad_lines=False)\ntest_df.head()"
        },
        {
            "cell_type": "code",
            "execution_count": 41,
            "metadata": {},
            "outputs": [
                {
                    "name": "stderr",
                    "output_type": "stream",
                    "text": "/opt/conda/envs/Python36/lib/python3.6/site-packages/sklearn/preprocessing/data.py:645: DataConversionWarning: Data with input dtype uint8, float64 were all converted to float64 by StandardScaler.\n  return self.partial_fit(X, y)\n/opt/conda/envs/Python36/lib/python3.6/site-packages/ipykernel/__main__.py:10: DataConversionWarning: Data with input dtype uint8, float64 were all converted to float64 by StandardScaler.\n"
                },
                {
                    "data": {
                        "text/plain": "array([[ 3.37365934e-01,  2.66479976e+00, -2.46831661e+00,\n         2.13703245e-01,  9.44090550e-01,  1.58113883e+00,\n        -4.08248290e-01],\n       [ 7.03145068e-01, -7.13778644e-01,  1.07370841e+00,\n         4.82633172e-01,  4.77498943e-01,  1.58113883e+00,\n        -4.08248290e-01],\n       [ 9.31757027e-01, -8.78587347e-01,  1.23870131e+00,\n         7.85179340e-01, -9.22275877e-01,  1.58113883e+00,\n        -4.08248290e-01],\n       [ 3.60227129e-01,  7.14563447e-01, -8.92254236e-02,\n        -3.57772849e-01,  6.89586037e-01,  1.58113883e+00,\n        -4.08248290e-01],\n       [-1.12575060e+00,  3.92401673e-04, -9.03545224e-01,\n        -1.13094639e+00,  1.09073363e-02, -6.32455532e-01,\n         2.44948974e+00]])"
                    },
                    "execution_count": 41,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "test_df['windex'] = np.where(test_df.WAB > 7, 'True', 'False')\ntest_df1 = test_df[test_df['POSTSEASON'].str.contains('F4|S16|E8', na=False)]\ntest_df1. head()\ntest_df1.groupby(['windex'])['POSTSEASON'].value_counts(normalize=True)\ntest_Feature = test_df1[['ADJOE','ADJDE','BARTHAG','EFG_O','EFG_D']]\ntest_Feature = pd.concat([test_Feature,pd.get_dummies(test_df1['POSTSEASON'])], axis=1)\ntest_Feature.drop(['S16'], axis = 1,inplace=True)\ntest_Feature.head()\ntest_X=test_Feature\ntest_X= preprocessing.StandardScaler().fit(test_X).transform(test_X)\ntest_X[0:5]"
        },
        {
            "cell_type": "code",
            "execution_count": 42,
            "metadata": {},
            "outputs": [
                {
                    "data": {
                        "text/plain": "array(['E8', 'E8', 'E8', 'E8', 'F4'], dtype=object)"
                    },
                    "execution_count": 42,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "test_y = test_df1['POSTSEASON'].values\ntest_y[0:5]"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "KNN"
        },
        {
            "cell_type": "code",
            "execution_count": 60,
            "metadata": {},
            "outputs": [
                {
                    "name": "stdout",
                    "output_type": "stream",
                    "text": "The Accuracy is:0.96 \nThe Jaccard Score is:0.96 \nand the F1-score is:0.96\n"
                }
            ],
            "source": "yhat = neigh.predict(test_X)\n\nprint(\"The Accuracy is:%.2f\"%accuracy_score(test_y,yhat),'\\n'\n    \"The Jaccard Score is:%.2f\"%metrics.jaccard_similarity_score(test_y,yhat),'\\n'\n      \"and the F1-score is:%.2f\"%metrics.f1_score(test_y,yhat,average='weighted')\n     )"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "Decision Tree"
        },
        {
            "cell_type": "code",
            "execution_count": 95,
            "metadata": {},
            "outputs": [
                {
                    "name": "stdout",
                    "output_type": "stream",
                    "text": "The Accuracy is:1.00 \n The Jaccard Score is:1.00 \n and the F1-score is:1.00\n"
                }
            ],
            "source": "Dtree = DecisionTreeClassifier(criterion='entropy',max_depth=2).fit(X_train,y_train)\n\nyhat_dt = Dtree.predict(test_X)\n\nprint(\"The Accuracy is:%.2f\"%accuracy_score(test_y,yhat_dt),'\\n',\n      \"The Jaccard Score is:%.2f\"%metrics.jaccard_similarity_score(test_y,yhat_dt),'\\n',\n      \"and the F1-score is:%.2f\"%metrics.f1_score(test_y,yhat_dt,average='weighted')\n     )"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "SVM"
        },
        {
            "cell_type": "code",
            "execution_count": 70,
            "metadata": {},
            "outputs": [
                {
                    "name": "stdout",
                    "output_type": "stream",
                    "text": "The Accuracy is:1.00 \nThe Jaccard Score is:1.00 \nand the F1-score is:1.00\n"
                }
            ],
            "source": "yhat_svm = clf.predict(test_X)\n\nprint(\"The Accuracy is:%.2f\"%accuracy_score(test_y,yhat_svm),'\\n'\n    \"The Jaccard Score is:%.2f\"%metrics.jaccard_similarity_score(test_y,yhat_svm),'\\n'\n      \"and the F1-score is:%.2f\"%metrics.f1_score(test_y,yhat_svm,average='weighted')\n     )"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "Logistic Regression"
        },
        {
            "cell_type": "code",
            "execution_count": 88,
            "metadata": {},
            "outputs": [
                {
                    "name": "stdout",
                    "output_type": "stream",
                    "text": "['F4' 'S16' 'S16' 'S16' 'S16' 'E8' 'S16' 'F4' 'S16' 'E8' 'S16' 'S16']\n"
                }
            ],
            "source": "print(y_val)"
        },
        {
            "cell_type": "code",
            "execution_count": 93,
            "metadata": {},
            "outputs": [
                {
                    "name": "stdout",
                    "output_type": "stream",
                    "text": "The Jaccard Score is:1.00 \nthe F1-score is:1.00 \nand the LogLos is:0.98\n"
                }
            ],
            "source": "yhat_LR = LR.predict(test_X)\n\nprint(\"The Jaccard Score is:%.2f\"%metrics.jaccard_similarity_score(test_y,yhat_LR),'\\n'\n      \"the F1-score is:%.2f\"%metrics.f1_score(test_y,yhat_LR,average='weighted'),'\\n'\n      \"and the LogLos is:%.2f\"%log_loss(y_val, yhat_prob)\n     )"
        },
        {
            "cell_type": "markdown",
            "metadata": {},
            "source": "# Report\nYou should be able to report the accuracy of the built model using different evaluation metrics:"
        },
        {
            "cell_type": "code",
            "execution_count": 98,
            "metadata": {},
            "outputs": [
                {
                    "data": {
                        "text/html": "<div>\n<style scoped>\n    .dataframe tbody tr th:only-of-type {\n        vertical-align: middle;\n    }\n\n    .dataframe tbody tr th {\n        vertical-align: top;\n    }\n\n    .dataframe thead th {\n        text-align: right;\n    }\n</style>\n<table border=\"1\" class=\"dataframe\">\n  <thead>\n    <tr style=\"text-align: right;\">\n      <th></th>\n      <th>Jaccard</th>\n      <th>F1-score</th>\n      <th>LogLoss</th>\n    </tr>\n    <tr>\n      <th>Algorithm</th>\n      <th></th>\n      <th></th>\n      <th></th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>KNN</th>\n      <td>0.957143</td>\n      <td>0.956147</td>\n      <td>NA</td>\n    </tr>\n    <tr>\n      <th>Decision Tree</th>\n      <td>1.000000</td>\n      <td>1.000000</td>\n      <td>NA</td>\n    </tr>\n    <tr>\n      <th>SVM</th>\n      <td>1.000000</td>\n      <td>1.000000</td>\n      <td>NA</td>\n    </tr>\n    <tr>\n      <th>LogisticRegression</th>\n      <td>1.000000</td>\n      <td>1.000000</td>\n      <td>0.97957</td>\n    </tr>\n  </tbody>\n</table>\n</div>",
                        "text/plain": "                     Jaccard  F1-score  LogLoss\nAlgorithm                                      \nKNN                 0.957143  0.956147       NA\nDecision Tree       1.000000  1.000000       NA\nSVM                 1.000000  1.000000       NA\nLogisticRegression  1.000000  1.000000  0.97957"
                    },
                    "execution_count": 98,
                    "metadata": {},
                    "output_type": "execute_result"
                }
            ],
            "source": "columns = ['Algorithm','Jaccard','F1-score','LogLoss']\n    \nknn = ['KNN',metrics.jaccard_similarity_score(test_y,yhat),\n       metrics.f1_score(test_y,yhat,average='weighted'),\n       'NA'\n      ]\nDecisionTree = ['Decision Tree',metrics.jaccard_similarity_score(test_y,yhat_dt),\n                metrics.f1_score(test_y,yhat_dt,average='weighted'),\n                'NA'\n               ]\nSVM = ['SVM',metrics.jaccard_similarity_score(test_y,yhat_svm),\n       metrics.f1_score(test_y,yhat_svm,average='weighted'),\n       'NA'\n      ]\nLogisticRegression = ['LogisticRegression',metrics.jaccard_similarity_score(test_y,yhat_LR),\n                      metrics.f1_score(test_y,yhat_LR,average='weighted'),\n                      log_loss(y_val, yhat_prob)\n                     ]\n\nreport = pd.DataFrame(columns=columns)\n\nfor i in range(0,len(columns)):\n    results = report.columns[i]\n    report[results]=[knn[i],DecisionTree[i],SVM[i],LogisticRegression[i]]\n    \nreport = report.set_index(['Algorithm'])\n    \nreport\n"
        },
        {
            "cell_type": "markdown",
            "metadata": {
                "button": false,
                "new_sheet": false,
                "run_control": {
                    "read_only": false
                }
            },
            "source": "<h2>Want to learn more?</h2>\n\nIBM SPSS Modeler is a comprehensive analytics platform that has many machine learning algorithms. It has been designed to bring predictive intelligence to decisions made by individuals, by groups, by systems \u2013 by your enterprise as a whole. A free trial is available through this course, available here: <a href=\"http://cocl.us/ML0101EN-SPSSModeler\">SPSS Modeler</a>\n\nAlso, you can use Watson Studio to run these notebooks faster with bigger datasets. Watson Studio is IBM's leading cloud solution for data scientists, built by data scientists. With Jupyter notebooks, RStudio, Apache Spark and popular libraries pre-packaged in the cloud, Watson Studio enables data scientists to collaborate on their projects without having to install anything. Join the fast-growing community of Watson Studio users today with a free account at <a href=\"https://cocl.us/ML0101EN_DSX\">Watson Studio</a>\n\n<h3>Thanks for completing this lesson!</h3>\n\n<h4>Author:  <a href=\"https://ca.linkedin.com/in/saeedaghabozorgi\">Saeed Aghabozorgi</a></h4>\n<p><a href=\"https://ca.linkedin.com/in/saeedaghabozorgi\">Saeed Aghabozorgi</a>, PhD is a Data Scientist in IBM with a track record of developing enterprise level applications that substantially increases clients\u2019 ability to turn data into actionable knowledge. He is a researcher in data mining field and expert in developing advanced analytic methods like machine learning and statistical modelling on large datasets.</p>\n\n<hr>\n\n<p>Copyright &copy; 2018 <a href=\"https://cocl.us/DX0108EN_CC\">Cognitive Class</a>. This notebook and its source code are released under the terms of the <a href=\"https://bigdatauniversity.com/mit-license/\">MIT License</a>.</p>"
        }
    ],
    "metadata": {
        "kernelspec": {
            "display_name": "Python 3.6",
            "language": "python",
            "name": "python3"
        },
        "language_info": {
            "codemirror_mode": {
                "name": "ipython",
                "version": 3
            },
            "file_extension": ".py",
            "mimetype": "text/x-python",
            "name": "python",
            "nbconvert_exporter": "python",
            "pygments_lexer": "ipython3",
            "version": "3.6.9"
        }
    },
    "nbformat": 4,
    "nbformat_minor": 4
}
