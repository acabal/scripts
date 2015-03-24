#!/bin/sh

usage(){
	fmt <<EOF
DESCRIPTION
	Add cover art to an OGG file.
	Adapted from mussync-tools by biapy: https://github.com/biapy/howto.biapy.com/blob/master/various/mussync-tools

USAGE
	ogg-cover-art [COVER-FILENAME] OGG-FILENAME
		Add cover art to OGG-FILENAME.  If COVER-FILENAME is not specified, 
		use cover.jpg, cover.jpeg, or cover.png located in the same folder
		as OGG-FILENAME.
EOF
	exit
}
die(){ printf "Error: ${1}\n" 1>&2; exit 1; }
require(){ command -v $1 > /dev/null 2>&1 || { suggestion=""; if [ ! -z "$2" ]; then suggestion=" $2"; fi; die "$1 is not installed.${suggestion}"; } }
if [ $# -eq 1 ]; then if [ "$1" = "--help" -o "$1" = "-h" ]; then usage; fi fi
#End boilerplate

require "vorbiscomment" "Try: apt-get install vorbis-tools"

if [ $# -eq 0 ]; then
	usage
fi

outputFile=""
imagePath=""

if [ $# -eq 1 ]; then
	outputFile="$1"
	dirName=$(dirname "$1")
	imagePath="${dirName}/cover.jpg"
	if [ ! -f "${imagePath}" ]; then
		imagePath="${dirName}/cover.jpeg"
		if [ ! -f "${imagePath}" ]; then
			imagePath="${dirName}/cover.png"
			if [ ! -f "${imagePath}" ]; then
				die "Couldn't find a cover image in ${dirName}."
			fi
		fi
	fi
fi

if [ $# -eq 2 ]; then
	outputFile="$2"
	imagePath="$1"
fi

if [ ! -f "${outputFile}" ]; then
	die "Couldn't find ogg file."
fi

if [ ! -f "${imagePath}" ]; then
	die "Couldn't find cover image."
fi

imageMimeType=$(file -b --mime-type "${imagePath}")

if [ "${imageMimeType}" != "image/jpeg" -a "${imageMimeType}" != "image/png" ]; then
	die "Cover image isn't a jpg or png image."
fi

oggMimeType=$(file -b --mime-type "${outputFile}")

if [ "${oggMimeType}" != "audio/x-vorbis+ogg" -a "${oggMimeType}" != "audio/ogg" ]; then
	die "Input file isn't an ogg file."
fi

#Export existing comments to file
commentsPath="$(mktemp -t "tmp.XXXXXXXXXX")"
vorbiscomment --list --raw "${outputFile}" > "${commentsPath}"

#Remove existing images
sed -i -e '/^metadata_block_picture/d' "${commentsPath}"

#Insert cover image from file

#metadata_block_picture format
#See: https://xiph.org/flac/format.html#metadata_block_picture
imageWithHeader="$(mktemp -t "tmp.XXXXXXXXXX")"
description=""

#Reset cache file
echo -n "" > "${imageWithHeader}"

#Picture type <32>
printf "0: %.8x" 3 | xxd -r -g0 >> "${imageWithHeader}"

#Mime type length <32>
printf "0: %.8x" $(echo -n "${imageMimeType}" | wc -c) | xxd -r -g0 >> "${imageWithHeader}"

#Mime type (n * 8)
echo -n "${imageMimeType}" >> "${imageWithHeader}"

#Description length <32>
printf "0: %.8x" $(echo -n "${description}" | wc -c) | xxd -r -g0 >> "${imageWithHeader}"

#Description (n * 8)
echo -n "${description}" >> "${imageWithHeader}"

#Picture with <32>
printf "0: %.8x" 0 | xxd -r -g0  >> "${imageWithHeader}"

#Picture height <32>
printf "0: %.8x" 0 | xxd -r -g0 >> "${imageWithHeader}"

#Picture color depth <32>
printf "0: %.8x" 0 | xxd -r -g0 >> "${imageWithHeader}"

#Picture color count <32>
printf "0: %.8x" 0 | xxd -r -g0 >> "${imageWithHeader}"

#Image file size <32>
printf "0: %.8x" $(wc -c "${imagePath}" | cut --delimiter=' ' --fields=1) | xxd -r -g0 >> "${imageWithHeader}"

#Image file
cat "${imagePath}" >> "${imageWithHeader}"

echo "metadata_block_picture=$(base64 --wrap=0 < "${imageWithHeader}")" >> "${commentsPath}"

#Update vorbis file comments
vorbiscomment --write --raw --commentfile "${commentsPath}" "${outputFile}"

#Delete temp files
rm "${imageWithHeader}"
rm "${commentsPath}"
