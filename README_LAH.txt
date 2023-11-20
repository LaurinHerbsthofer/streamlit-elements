Here are the steps I took to integrate the nivo/boxplot into streamlit-elements on a linux machine.

1) I used grep -rni "ResponsiveBar" and grep -rni "ResponsiveBoxplot" to find that the boxplot was not included while the bar plot was.
2) Using the output of the first step, I added the boxplot into the the .tsx file that specifies what nivo functions are needed:
./streamlit_elements/frontend/components/modules/charts/Nivo.tsx:8:  BoxPlot: dynamic(() => import("@nivo/boxplot").then(m => m.ResponsiveBoxPlot), { loading: ElementsLoading, ssr: false }),


3) adding __version__ = "0.1.0" to the file streamlit_elements/__init__.py so that the pip intall -e command of the Makefile goes through.
4) Install npm
5) Adding nivo/boxplot requirements to streamlit_elements/frontend/package.json: "@nivo/boxplot": "^0.84.0",
6) Run the Makefile using the make command

7) doing another grep -rni "ResponsiveBoxplot" and comparing the output to grep -rni "ResponsiveBar" looked promising.
8) the output created by the makefile seemed similar to the code found online: https://www.npmjs.com/package/@nivo/boxplot?activeTab=code

9) next we have to actually build all the source files running make build (check the Makefile for options). To fix a pyenv error I had to add the local python verison to pyenv using: pyenv global 3.9.18
10) after doing another grep -rni "responsiveboxplot" we see that all .js source files and binaries now exist

11) from an actual project that uses streamlit-elements (e.g. streamlit-gallery) we can find out what .js files are needed by first making the repo work (i.e. installing all python dependencies from Pipfile), and then doing a project-wide search in PyCharm (double shift) and searching for responsivebar. This will show that the necessary javascript code is located e.g. in /home/laurin/.local/share/virtualenvs/streamlit-gallery-lypl7mn3/lib/python3.10/site-packages/streamlit_elements/frontend/build/_next/static/chunks/2491.b121941625392a8b.js
12) this frontend/build folder was now also created by our make command, we just have to get the data over to our environment to test it. Ideally, we simply push our code to a github repo and install streamlit-elements with our modifications from there to a project.

12b) e.g. cp ~/github/streamlit-elements/streamlit_elements/ ~/.local/share/virtualenvs/streamlit-gallery-lypl7mn3/lib/python3.10/site-packages/streamlit_elements -r

10) to test it, I used the code from streamlit-gallery (ToDo)

