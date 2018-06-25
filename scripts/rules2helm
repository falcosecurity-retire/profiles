#!/bin/bash

echo "customRules:"
for rules_file in "$@"
do
  echo -e "  $(basename $rules_file): |-"
  while IFS= read -r line
  do
    echo -e "    $line"
  done <"$rules_file"
done
