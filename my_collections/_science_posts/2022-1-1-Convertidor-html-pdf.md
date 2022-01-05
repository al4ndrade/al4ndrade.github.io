---
layout: post
title: "Convertidor de HTML a pdf" 
description: "Un pequeño código en python para convertir páginas html en pdf"
image: images/sci/htmlpdf/htmlpdf.jpg
date: 2022-1-2
---

El siguiente código que muestro a continuación, es uno de los códigos que utilizo con bastante frecuencia. Lo escribí por la necesidad de enviar comunicados en formato pdf, lo cual con las herramientas adecuadas no es nada complicado, sin embargo, no tengo un procesador de texto que convierta a pdf, por lo tanto me di a la tarea de crear este convertidor y dado que me es muy sencillo escribir en html, me ha funcionado a la perfección.

## Requerimientos

- pdfkit
- wkhtmltopdf

### Desde la terminal:

1. Instalar python-pdfkit:
<code> $ pip install pdfkit </code>

2. Instalar wkhtmltopdf:

	- Debian/Ubuntu:
		<code> $ sudo apt-get install wkhtmltopdf </code>
	- macOS:
		<code> $ brew install homebrew/cask/wkhtmltopdf </code>


Al parecer las versiones de debian y ubuntu redujeron su funcionalidad. Por lo tanto es mejor instalar desde el sitio https://wkhtmltopdf.org


## Mi Código


	import pdfkit

	options = {
		'enable-local-file-access': None,
		'margin-right': '5mm',
		'margin-left': '5mm',
		'margin-top': '5mm',
		'margin-bottom': '5mm'
	}

	htmlfilename = input('Escribe el nombre del html a convertir: ')
	pdfname = input('Escribe el nombre del pdf de salida: ')

	filename = htmlfilename + '.html'
	print(filename)
	pdfn = pdfname + '.pdf'
	print(pdfn)

	with open(filename, 'r') as f:
		pdfkit.from_file(f, pdfn, options=options)


La siguiente línea es importante para poder utilizar imágenes que se encuentren en nuestra computadora, de otra forma, aunque en el html veamos estas imágenes, al momento de convertir a pdf, no aparecerán.

	'enable-local-file-access': None,


En mi canal pueden ver un explicación más detallada de este proceso y además envío de mails.

<div class='align-center'>
<iframe width="560" height="315" src="https://www.youtube.com/embed/Qt5xmnOPFEo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>