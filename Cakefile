###
Presents - A web library to show video/slides presentations

Copyright (C) 2011 Federico Fissore

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
###

exec = require('child_process').exec

task 'build', ->
  exec 'mkdir -p build', (err) ->
    console.log err if err
  exec 'rm -rf build/*', (err) ->
    console.log err if err
  exec 'coffee -o build -j Presentz.js -c src/Html5.coffee src/Presentz.coffee src/Video.coffee src/Vimeo.coffee', (err) ->
    console.log err if err
  exec 'coffee -o build -j DynPresentation.js -c src/DynPresentation.coffee', (err) ->
    console.log err if err
