ssh-user = softlang@linux.uni-koblenz.de
www-dir = /home/softlang/www/mega

all:
	pdflatex report
	pdflatex report

clean:
	rm -f report.aux
	rm -f report.log
	rm -f report.toc
	rm -f report.pdf

upload:
	scp -r -p report.pdf ${decks} ${ssh-user}:${www-dir}/oxModels12.pdf
