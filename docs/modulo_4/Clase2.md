{
  "cells": [
    {
      "cell_type": "markdown",
      "id": "d62f3fac",
      "metadata": {
        "id": "d62f3fac"
      },
      "source": [
        "# Clase 2: Librería Chainladder\n",
        "\n",
        "<a href=\"https://colab.research.google.com/github/andesanalytics/python/blob/main/docs/modulo_4/Clase2.ipynb\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>\n",
        "\n",
        "## Introducción a Chainladder\n",
        "\n",
        "+ La biblioteca `chainladder` de Python es una herramienta utilizada en el análisis actuarial,\n",
        "principalmente en el ámbito de seguros.\n",
        "\n",
        "+ Su principal función es ayudar en la estimación de reservas de siniestros ocurridos y no reportados.\n",
        "\n",
        "+ La librería posee multiples técnicas de proyección, que se basan en el análisis de triángulos de desarrollo de pérdidas, que\n",
        "permiten proyectar los siniestros futuros basados en los datos históricos\n"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "891e523f",
      "metadata": {
        "id": "891e523f"
      },
      "source": [
        "## Instalación y Configuración\n",
        "\n",
        "Para instalar la biblioteca `chainladder`, ejecuta el siguiente comando en tu entorno Python:\n"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 5,
      "id": "1c5f2e20",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "1c5f2e20",
        "outputId": "72e25ec0-0e9e-4d41-ca7e-c0404417ee3c"
      },
      "outputs": [
        {
          "name": "stdout",
          "output_type": "stream",
          "text": [
            "Collecting chainladder\n",
            "  Downloading chainladder-0.8.18-py3-none-any.whl (1.3 MB)\n",
            "\u001b[2K     \u001b[90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\u001b[0m \u001b[32m1.3/1.3 MB\u001b[0m \u001b[31m8.6 MB/s\u001b[0m eta \u001b[36m0:00:00\u001b[0m\n",
            "\u001b[?25hRequirement already satisfied: pandas>=0.23 in /usr/local/lib/python3.10/dist-packages (from chainladder) (1.5.3)\n",
            "Requirement already satisfied: scikit-learn>=0.23 in /usr/local/lib/python3.10/dist-packages (from chainladder) (1.2.2)\n",
            "Requirement already satisfied: numba>0.54 in /usr/local/lib/python3.10/dist-packages (from chainladder) (0.58.1)\n",
            "Collecting sparse>=0.9 (from chainladder)\n",
            "  Downloading sparse-0.15.1-py2.py3-none-any.whl (116 kB)\n",
            "\u001b[2K     \u001b[90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\u001b[0m \u001b[32m116.3/116.3 kB\u001b[0m \u001b[31m12.7 MB/s\u001b[0m eta \u001b[36m0:00:00\u001b[0m\n",
            "\u001b[?25hRequirement already satisfied: matplotlib in /usr/local/lib/python3.10/dist-packages (from chainladder) (3.7.1)\n",
            "Collecting dill (from chainladder)\n",
            "  Downloading dill-0.3.8-py3-none-any.whl (116 kB)\n",
            "\u001b[2K     \u001b[90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\u001b[0m \u001b[32m116.3/116.3 kB\u001b[0m \u001b[31m14.1 MB/s\u001b[0m eta \u001b[36m0:00:00\u001b[0m\n",
            "\u001b[?25hRequirement already satisfied: patsy in /usr/local/lib/python3.10/dist-packages (from chainladder) (0.5.6)\n",
            "Requirement already satisfied: llvmlite<0.42,>=0.41.0dev0 in /usr/local/lib/python3.10/dist-packages (from numba>0.54->chainladder) (0.41.1)\n",
            "Requirement already satisfied: numpy<1.27,>=1.22 in /usr/local/lib/python3.10/dist-packages (from numba>0.54->chainladder) (1.25.2)\n",
            "Requirement already satisfied: python-dateutil>=2.8.1 in /usr/local/lib/python3.10/dist-packages (from pandas>=0.23->chainladder) (2.8.2)\n",
            "Requirement already satisfied: pytz>=2020.1 in /usr/local/lib/python3.10/dist-packages (from pandas>=0.23->chainladder) (2023.4)\n",
            "Requirement already satisfied: scipy>=1.3.2 in /usr/local/lib/python3.10/dist-packages (from scikit-learn>=0.23->chainladder) (1.11.4)\n",
            "Requirement already satisfied: joblib>=1.1.1 in /usr/local/lib/python3.10/dist-packages (from scikit-learn>=0.23->chainladder) (1.3.2)\n",
            "Requirement already satisfied: threadpoolctl>=2.0.0 in /usr/local/lib/python3.10/dist-packages (from scikit-learn>=0.23->chainladder) (3.3.0)\n",
            "Requirement already satisfied: contourpy>=1.0.1 in /usr/local/lib/python3.10/dist-packages (from matplotlib->chainladder) (1.2.0)\n",
            "Requirement already satisfied: cycler>=0.10 in /usr/local/lib/python3.10/dist-packages (from matplotlib->chainladder) (0.12.1)\n",
            "Requirement already satisfied: fonttools>=4.22.0 in /usr/local/lib/python3.10/dist-packages (from matplotlib->chainladder) (4.49.0)\n",
            "Requirement already satisfied: kiwisolver>=1.0.1 in /usr/local/lib/python3.10/dist-packages (from matplotlib->chainladder) (1.4.5)\n",
            "Requirement already satisfied: packaging>=20.0 in /usr/local/lib/python3.10/dist-packages (from matplotlib->chainladder) (23.2)\n",
            "Requirement already satisfied: pillow>=6.2.0 in /usr/local/lib/python3.10/dist-packages (from matplotlib->chainladder) (9.4.0)\n",
            "Requirement already satisfied: pyparsing>=2.3.1 in /usr/local/lib/python3.10/dist-packages (from matplotlib->chainladder) (3.1.1)\n",
            "Requirement already satisfied: six in /usr/local/lib/python3.10/dist-packages (from patsy->chainladder) (1.16.0)\n",
            "Installing collected packages: dill, sparse, chainladder\n",
            "Successfully installed chainladder-0.8.18 dill-0.3.8 sparse-0.15.1\n"
          ]
        }
      ],
      "source": [
        "pip install chainladder"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "62fe5f27",
      "metadata": {
        "id": "62fe5f27"
      },
      "source": [
        "Una vez instalada, puedes importarla y verificar su versión para asegurarte de que está correctamente instalada:"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 6,
      "id": "c9b898f6",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "c9b898f6",
        "outputId": "8a84eb21-5ccf-4603-a435-36aa803c356a"
      },
      "outputs": [
        {
          "name": "stdout",
          "output_type": "stream",
          "text": [
            "0.8.18\n"
          ]
        }
      ],
      "source": [
        "import chainladder as cl\n",
        "print(cl.__version__)"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "69c38534",
      "metadata": {
        "id": "69c38534"
      },
      "source": [
        "## Manejo de Datos: Propiedades Básicas\n",
        "+ El paquete chainladder tiene su propia estructura de datos `:class:Triangle` que se comporta de manera muy similar a un `DataFrame` de ``pandas``\n",
        "+ Por qué no ``pandas``? Principalmente porque podemos manejar multiples triángulos al mismo tiempo, junto con la eficiencia en memoria\n",
        "+ La estructura `Triangle` es una estructura de datos en 4D con ejes etiquetados. Estos ejes son ``index``, ``columns``, ``origin``, ``development``\n",
        "\n",
        "\n",
        "``index`` (axis 0): Es la agrupación mínima a la cual se requiera ver los datos (por ejemplo por producto, tipo de negocio, ramo fecu). Tal como en ``pandas.multiIndex``, se pueden tener más de una columa dentro de `index`\n",
        "\n",
        "``columns`` (axis 1): Son el o los valores numéricos que deseas agrupar en triángulo. Si bien el valor obvio a acumular es el de siniestros pagados, también puedes tener otros triángulos como el número de siniestros, o los siniestros pendientes por ejemplo.\n",
        "\n",
        "``origin`` (axis 2): Es el eje vertical dentro de un triángulo tradicional. Corresponde al mes, trimestre, semestre o año en el cual ocurrió el siniestro. La periodicidad se puede definir por el usuario.\n",
        "\n",
        "``development`` (axis 3): Es el eje horizontal del triángulo. Representa el periodo en que el siniestro fue reportado. Al igual que el eje `origin` se puede tomar la periodicidad mensual, trimestral, semestral, anual.\n",
        "\n",
        "### ¿Cómo se trabaja con una clase ``Triangle``?\n",
        "+ Independiente de poseer 4 dimensiones, esta clase permite que se interactúe con él de la misma manera que en `pandas`. Puedes usar los ejes `index` y `columns` de la misma manera que en un `DataFrame`, donde cada elemento será un triángulo tradicional. La siguiente imagen muestra como sería la estructura de la clase.\n",
        "\n",
        "<img src=\"https://drive.google.com/uc?export=view&id=1Cv8pnt4WqfjfVv7FSmhgBng7oT70g5_i\" width = \"100\" align=\"center\"/>\n",
        "\n",
        "\n",
        "### ¿Cómo se construye una clase ``Triangle``?\n",
        "\n",
        "+ Una clase `Triangle` se crea a partir de un `DataFrame` de `pandas`, el cuál debe poseer como mínimo las siguientes columnas.\n",
        "    * Dos columnas de tipo fecha que representen los ejes ``origin`` y ``development``.\n",
        "    * Una columna numérica que represente el eje ``columns``\n",
        "\n"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "4270bd68",
      "metadata": {
        "id": "4270bd68"
      },
      "outputs": [],
      "source": [
        "import pandas as pd\n",
        "raa_df = pd.read_csv(\"https://raw.githubusercontent.com/casact/chainladder-python/master/chainladder/utils/data/raa.csv\")\n",
        "raa_df.head(10)"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "a858ac34",
      "metadata": {
        "id": "a858ac34"
      },
      "source": [
        "Vemos que dataframe cargado posee las columnas mínimas requeridas.\n",
        "La forma para obtener una clase de tipo `Triangle` se obtiene con el método `Triangle`"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "e635385a",
      "metadata": {
        "id": "e635385a"
      },
      "outputs": [],
      "source": [
        "raa = cl.Triangle(raa_df,origin=\"origin\",development=\"development\",columns=\"values\",cumulative=True)\n",
        "print(raa.shape)\n",
        "raa"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "3c64d761",
      "metadata": {
        "id": "3c64d761"
      },
      "source": [
        "+ Como se puede observar, la información fue agrupada como triángulo.\n",
        "+ Las dimensiones del triángulo indican 4 dimensiones, que corresponden a los ejes anteriormente comentados\n",
        "+ La visualización del triángulo cambiará si nuestra data contiene más de un valor en el eje ``columns`` o más de un valor en el eje `index`\n",
        "\n",
        "### ``Triangle`` en 4D\n",
        "Ahora revisemos el siguiente triangulo cargado de los datos de muestra de la librería"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "51050676",
      "metadata": {
        "id": "51050676"
      },
      "outputs": [],
      "source": [
        "triangle = cl.load_sample('CLRD')\n",
        "print(triangle.shape)\n",
        "triangle"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "48b052e5",
      "metadata": {
        "id": "48b052e5"
      },
      "source": [
        "Vemos que la forma de mostrar la data cambia, debido a que el eje ``index`` posee 775 diferentes niveles, mientras que el eje `columns` posee 6 distintas columnas para formar triángulos.\n",
        "\n",
        "### Parametro `cumulative`\n",
        "+ El parámetro ``cumulative`` indica si los datos que entregamos en el eje `columns` corresponden a datos acumulativos a la fecha o son los datos específicos de la fecha.\n",
        "+ Es completamente opcional. Si no se especifica, el Triángulo inferirá su estado ``cumulative/incremental`` en el punto en el que llame a los métodos ``cum_to_incr`` o ``incr_to_cum`` (que sirven para pasar de un estado a otro)\n",
        "+ El método `is_cumulative` nos permite saber si un triangulo tiene su parametro `cumulative` en `True` o `False`"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "eadeacda",
      "metadata": {
        "id": "eadeacda"
      },
      "outputs": [],
      "source": [
        "raa.cum_to_inc()"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "7825dfb0",
      "metadata": {
        "id": "7825dfb0"
      },
      "source": [
        "### Valoración vs Desarrollo\n",
        "Si bien la mayoría de los estimadores que utilizan triángulos esperan que el período de desarrollo se exprese como una edad sobre el periodo de origen (diagonal de abajo hacia arriba), es posible transformar un triángulo en un triángulo de valoración, donde los períodos de desarrollo se convierten en períodos de valoración (es decir con la diagonal de arriba hacia abajo) con el método `dev_to_val`\n",
        "\n",
        "Los triangulos poseen también la propiedad `is_val_tri` que indica si el triangulo está en modo valoración (diagonal de arriba hacia abajo)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "02471cef",
      "metadata": {
        "id": "02471cef"
      },
      "outputs": [],
      "source": [
        "raa.dev_to_val()\n",
        "raa.is_val_tri"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "e3dc9403",
      "metadata": {
        "id": "e3dc9403"
      },
      "source": [
        "### Granularidad del Triangulo\n",
        "Si el triángulo posee en sus ejes `origin` o `development` periodicidades más frecuentes a una anual (mensual, trimestral), se puede facilmente pasar a una granularidad mayor con el metodo `grain`. Este método reconoce las siguientes granularidades.\n",
        "+ Anual: Yearly (Y)\n",
        "+ Trimestral: Quarterly (Q)\n",
        "+ Mensual: Monthly (M)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "e49d794c",
      "metadata": {
        "id": "e49d794c"
      },
      "outputs": [],
      "source": [
        "# CARGAMOS DATA DE PRUEBA CON LA FUNCION load_sample()\n",
        "df_quarterly=cl.load_sample('quarterly')\n",
        "df_quarterly\n",
        "df_quarterly.grain('OYDY')"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "b6b829ab",
      "metadata": {
        "id": "b6b829ab"
      },
      "source": [
        "### Link Ratios\n",
        "Los factores de desarrollo por cada periodo origen-desarrollo (también conocidos en inglés como *link ratios*) pueden ser visualizados con la propiedad ``link_ratio``. Los triángulos además tienen un método llamado ``heatmap`` que puedes llamar opcionalmente para darle formatos a las salidas del triángulo. Este metodo requiere de IPython/Jupyter notebook para ejecutarse."
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "827b5df7",
      "metadata": {
        "id": "827b5df7"
      },
      "outputs": [],
      "source": [
        "triangle_abc = cl.load_sample('abc')\n",
        "triangle_abc\n",
        "triangle_abc.link_ratio\n",
        "triangle_abc.link_ratio.heatmap()"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "826c07e3",
      "metadata": {
        "id": "826c07e3"
      },
      "source": [
        "## Sintaxis Tipo Pandas\n",
        "### Filtros y Slicing\n",
        "Podemos filtrar y seleccionar triángulos de menor tamaño al igual que en pandas. Funciones como `iloc` o `loc` aplican de la misma manera.\n",
        "\n",
        "**Recordar que bajo la lógica de esta librería el eje `index` vendría siendo el eje de filas y el eje `columns` el de columnas en pandas.**"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "dc205fe7",
      "metadata": {
        "id": "dc205fe7"
      },
      "outputs": [],
      "source": [
        "# Cargamos data con load_sample\n",
        "clrd = cl.load_sample('clrd')\n",
        "# Slicing con iloc\n",
        "clrd.iloc[0,1]\n",
        "# Filtros del eje ``index`` mediante una condicion\n",
        "clrd[clrd['LOB']=='othliab']\n",
        "# Filtro del eje `columns` mediante una selecion de columnas\n",
        "clrd['EarnedPremDIR']"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "434bf503",
      "metadata": {
        "id": "434bf503"
      },
      "source": [
        "### Virtual Columns\n",
        "Hay casos en los que queremos posponer los cálculos, podemos crear columnas \"virtuales\" que pospongan los cálculos cuando sea necesario. Estas columnas se pueden crear envolviendo una columna normal en una función. Las expresiones lambda funcionan como una representación ordenada de columnas virtuales."
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "54573fcf",
      "metadata": {
        "id": "54573fcf"
      },
      "outputs": [],
      "source": [
        "clrd = cl.load_sample('clrd')\n",
        "# A physical column with immediate evaluation\n",
        "clrd['PaidLossRatio'] = clrd['CumPaidLoss'] / clrd['EarnedPremDIR']\n",
        "clrd.sum()['PaidLossRatio']\n",
        "# A virtual column with deferred evaluation\n",
        "clrd['PaidLossRatio'] = lambda clrd : clrd['CumPaidLoss'] / clrd['EarnedPremDIR']\n",
        "# Good - Defer loss ratio calculation until after summing premiums and losses\n",
        "clrd.sum()['PaidLossRatio']"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "43d6fc27",
      "metadata": {
        "id": "43d6fc27"
      },
      "source": [
        "### Agregaciones\n",
        "Al igual que los pandas, puedes agregar múltiples triángulos dentro de un Triángulo usando ``sum()`` que opcionalmente puede combinarse con ``groupby()``."
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "8e824127",
      "metadata": {
        "id": "8e824127"
      },
      "outputs": [],
      "source": [
        "clrd = cl.load_sample('clrd')\n",
        "clrd.sum()\n",
        "clrd.groupby('LOB').sum()"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "13baa1a5",
      "metadata": {
        "id": "13baa1a5"
      },
      "source": [
        "* Por defecto, la agregación se aplicará al primer eje con una longitud mayor que 1.\n",
        "* Alternativamente, puede especificar el eje usando el argumento ``axis`` del método agregado.\n",
        "\n",
        "### Convertir a ``DataFrame``\n",
        "Cuando un triángulo se presenta con un único nivel del eje ``index`` y una sola columna (eje `column`), se convierte en un objeto 2D (En un triángulo como vimos al comienzo). Como tal, su formato de visualización cambia a uno similar a un marco de datos. Estos triángulos 2D se pueden convertir fácilmente en un marco de datos de pandas utilizando el método ``to_frame``. A partir de este punto, los resultados se pueden operar directamente en pandas.\n",
        "\n",
        "La funcionalidad ``to_frame`` funciona cuando un Triángulo se divide en dos ejes cualesquiera y no se limita solo a `index` y `column`"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "50e5e936",
      "metadata": {
        "id": "50e5e936"
      },
      "outputs": [],
      "source": [
        "clrd = cl.load_sample('clrd')\n",
        "clrd\n",
        "clrd[clrd['LOB']=='ppauto']['CumPaidLoss'].sum().to_frame()\n",
        "clrd['CumPaidLoss'].groupby('LOB').sum().latest_diagonal.to_frame()\n"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "123037de",
      "metadata": {
        "id": "123037de"
      },
      "source": [
        "## Desarrollo de Triangulos\n",
        "Para realizar esto utilizamos `Development`, que permite la selección de patrones de desarrollo de pérdidas. Muchas de las técnicas típicas de promediación están disponibles en esta clase: simple, de volumen y de regresión a través del origen. Además, Desarrollo incluye patrones para permitir una exclusión precisa de las relaciones de enlace del cálculo de LDF.\n",
        "\n",
        "**Alternativamente, puedes proporcionar una lista para parametrizar cada período de desarrollo por separado. Debes tener en consideración que la lista debe tener la misma longitud que el eje de desarrollo de sus triángulos ``link_ratio``**"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "31d0c072",
      "metadata": {},
      "outputs": [],
      "source": [
        "raa = cl.load_sample('raa')\n",
        "cl.Development(average='simple')\n",
        "len(raa.link_ratio.development)\n",
        "cl.Development(average=['volume']+['simple']*8)"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "5ae1b46d",
      "metadata": {},
      "source": [
        "### Omitiendo Link Ratios\n",
        "Hay varios argumentos para eliminar celdas individuales del triángulo, así como para excluir períodos de valoración completos o máximos y mínimos. Se permite cualquier combinación de los argumentos de \"drop\".\n",
        "\n",
        "**Cuando se utiliza ``drop``, se debe hacer referencia a la edad más temprana de ``link_ratio``. Por ejemplo, utilice 12 para eliminar la proporción 12-24.**"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "68fff753",
      "metadata": {},
      "outputs": [],
      "source": [
        "cl.Development(drop_high=[True]*5+[False]*4, drop_low=[True]*5+[False]*4).fit(raa)\n",
        "cl.Development(drop_valuation='1985').fit(raa)\n",
        "cl.Development(drop=[('1985', 12), ('1987', 24)]).fit(raa)\n",
        "cl.Development(drop=('1985', 12), drop_valuation='1988').fit(raa)"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "37ae8039",
      "metadata": {},
      "source": [
        "### Transformación de Triangulos\n",
        "Al transformar un Triángulo, recibirás una copia del triángulo original junto con las propiedades ajustadas del estimador `Development`. \n",
        "\n",
        "Mientras que el Triángulo original contiene todas los link ratios, la versión transformada reconoce cualquier omisión que especifique."
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "28a75848",
      "metadata": {},
      "outputs": [],
      "source": [
        "triangle = cl.load_sample('raa')\n",
        "dev = cl.Development(drop=('1982', 12), drop_valuation='1988')\n",
        "transformed_triangle = dev.fit_transform(triangle)\n",
        "transformed_triangle.ldf_"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "f1788afa",
      "metadata": {},
      "outputs": [],
      "source": [
        "transformed_triangle.link_ratio.heatmap()"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "a486f947",
      "metadata": {},
      "source": [
        "Al desacoplar los métodos de ajuste y transformación, podemos aplicar nuestro estimador de desarrollo a nuevos datos. Este es un patrón común en ``scikit-learn``. En este ejemplo generamos patrones de desarrollo a nivel industrial y aplicamos esos patrones a empresas individuales."
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "cf03343d",
      "metadata": {},
      "outputs": [],
      "source": [
        "clrd = cl.load_sample('clrd')\n",
        "clrd = clrd[clrd['LOB']=='wkcomp']['CumPaidLoss']\n",
        "# Summarize Triangle to industry level to estimate patterns\n",
        "dev = cl.Development().fit(clrd.sum())\n",
        "# Apply Industry patterns to individual companies\n",
        "dev.transform(clrd)"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "a7628589",
      "metadata": {},
      "source": [
        "El estimador ``DevelopmentConstant`` simplemente le permite codificar patrones de desarrollo en un Estimador de desarrollo. Un ejemplo común sería incluir un conjunto de patrones de desarrollo de la industria en su flujo de trabajo que no se estimen directamente a partir de ninguno de sus propios datos."
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "9d9a02fb",
      "metadata": {},
      "outputs": [],
      "source": [
        "triangle = cl.load_sample('ukmotor')\n",
        "patterns={12: 2, 24: 1.25, 36: 1.1, 48: 1.08, 60: 1.05, 72: 1.02}\n",
        "cl.DevelopmentConstant(patterns=patterns, style='ldf').fit(triangle).ldf_"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "f4f25b2d",
      "metadata": {},
      "source": [
        "## Estimadores de cola\n",
        "LaS pérdidas no observadas más allá del borde de un Triángulo puedeN ser sustancialES y es una parte necesaria del análisis de un actuario. Esto es particularmente cierto para las líneas de cola larga, más comunes en seguros comerciales o niveles excesivos de pérdidas (seguros de reembolsos de gastos médicos de alto costo).\n",
        "\n",
        "Con todas las estimaciones de cola, estamos extrapolando más allá de los datos conocidos, lo que conlleva sus propios desafíos. Tiende a ser más difícil validar los supuestos cuando se realiza una estimación de cola. Además, muchas de las técnicas conllevan un alto grado de sensibilidad a los supuestos que se elegirán al realizar el análisis.\n",
        "\n",
        "Sin embargo, es una parte necesaria del conjunto de herramientas del actuario para estimar los pasivos de reserva.\n",
        "\n",
        "### Conceptos básicos y puntos en común\n",
        "+ Al igual que con la familia de estimadores `Development`, el módulo ``Tail`` proporciona una variedad de transformadores de cola que permiten la extrapolación de patrones de desarrollo más allá del final del triángulo. \n",
        "+ Al ser transformadores, podemos esperar que cada estimador tenga un método ``fit`` y otro método `transform `. \n",
        "+ El método `transform` nos devuelve nuestro Triángulo con parámetros estimados adicionales para incorporar nuestra revisión de cola en nuestras estimaciones de IBNR.\n",
        "+ Cada estimador ``tail`` produjo un atributo ``tail_`` que representa la estimación puntual de la cola del Triángulo.\n",
        "\n",
        "Veamos el primer ejemplo de una estimación con un valor asignado arbitrariamente."
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "cb60e542",
      "metadata": {},
      "outputs": [],
      "source": [
        "triangle = cl.load_sample('genins')\n",
        "tail = cl.TailConstant(1.10).fit_transform(triangle)\n",
        "tail.tail_"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "9aed6b14",
      "metadata": {},
      "source": [
        "**Al ajustar un estimador ``tail`` sin especificar primero un estimador ``Development``, chainladder asume un desarrollo de triangulos con el parametro `volume-weighted`. Para anular esto, primero debes declarar transformar tu triángulo con un transformador de desarrollo.**\n",
        "\n",
        "### Estimador `TailCurve`"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "fa8302f5",
      "metadata": {},
      "outputs": [],
      "source": [
        "triangle = cl.load_sample('quarterly')['paid']\n",
        "tail = cl.TailCurve().fit(triangle)\n",
        "# Slice just the tail entries\n",
        "tail.cdf_[~tail.ldf_.development.isin(triangle.link_ratio.development)]"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "1b8b5d39",
      "metadata": {},
      "source": [
        "### Comienzo de la Estimación\n",
        "Por defecto, los estimadores de cola se asignan a la edad de desarrollo más antigua del Triángulo. En la práctica, los últimos factores de desarrollo conocidos de un Triángulo pueden ser poco confiables y se prefiere unir la cola antes y usarla como mecanismo de suavizado. Todos los estimadores de cola tienen un parámetro ``attachment_age``  que le permite seleccionar la edad de desarrollo a la que se unirá la cola."
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "23a2cccf",
      "metadata": {},
      "outputs": [],
      "source": [
        "triangle = cl.load_sample('genins')\n",
        "unsmoothed = cl.TailCurve().fit(triangle).ldf_\n",
        "smoothed = cl.TailCurve(attachment_age=24).fit(triangle).ldf_\n",
        "pd.concat((\n",
        "    unsmoothed.T.iloc[:, 0].rename('Unsmoothed'),\n",
        "    smoothed.T.iloc[:, 0].rename('Age 24+ Smoothed')),\n",
        "    axis=1).plot(marker='o', title='Selected Link Ratio', ylabel='LDF');"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "99ed97d9",
      "metadata": {},
      "source": [
        "### Periodo de Proyección\n",
        "Independientemente de dónde se coloque una cola, habrá patrones incrementales hasta al menos un año después del final del Triángulo. Surgen casos en los que se desea modelar  en un horizonte temporal más largo. Para estos casos, es posible modificar el período de proyección (en meses) de todos los estimadores de cola especificando la cantidad de meses que desea extender sus patrones (dentro del parametro `projection_period`)."
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "51840c1d",
      "metadata": {},
      "outputs": [],
      "source": [
        "# Extend tail run-off 4 years past end of Triangle.\n",
        "tail = cl.TailCurve(projection_period=12*4).fit(triangle)\n",
        "tail.ldf_[~tail.ldf_.development.isin(triangle.link_ratio.development)]"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "2cfb117e",
      "metadata": {},
      "source": [
        "### Tasa de Caida\n",
        "Se puede controlar la tasa de caída de su cola. También está disponible un parámetro exponencial `decay`"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "84c2fe13",
      "metadata": {},
      "outputs": [],
      "source": [
        "tail = cl.TailConstant(tail=1.05, decay=0.95).fit_transform(triangle)\n",
        "tail.ldf_\n",
        "tail.tail_"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "5d95b8cf",
      "metadata": {},
      "source": []
    },
    {
      "cell_type": "markdown",
      "id": "3a11efc4",
      "metadata": {},
      "source": []
    },
    {
      "cell_type": "markdown",
      "id": "26d2189b",
      "metadata": {},
      "source": []
    },
    {
      "cell_type": "markdown",
      "id": "1047086b",
      "metadata": {},
      "source": []
    },
    {
      "cell_type": "markdown",
      "id": "5b51141f",
      "metadata": {},
      "source": []
    },
    {
      "cell_type": "markdown",
      "id": "dee82ba2",
      "metadata": {
        "id": "dee82ba2"
      },
      "source": []
    }
  ],
  "metadata": {
    "colab": {
      "provenance": []
    },
    "kernelspec": {
      "display_name": "Python 3",
      "name": "python3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "nbformat": 4,
  "nbformat_minor": 5
}
