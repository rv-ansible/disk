# disk role

- name: Check bindings
  shell: "cat /etc/multipath/bindings | tr -s '\n' '\n' | grep -v '#' > {{pre_check_tmp}}/temp43_007.log || true" 
 
- name: List the bindings 
  shell: "tail -n 9 {{pre_check_tmp}}/temp43_007.log > {{pre_check_tmp}}/temp44_008.log || true" 
    
- name: Register the bindings    
  shell: "cat {{pre_check_tmp}}/temp44_008.log | awk 'NR=={{item}}{print $1}' || true"
  register: w
  with_items: '{{numbers}}'
      
- name: Save the raw prefix
  set_fact: raw_prefix='mpath'
  when: item.stdout|regex_search('mpath')
  with_items: "{{w.results}}"
  until: raw_prefix=='mpath'
