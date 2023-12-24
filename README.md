#   SysAdmin macOS bootstrapper

To save our time when setting up a new Mac,
and to standardize our weaponry in a never ending war
against alerts and incidents,
let's use Ansible to bootstrap our workstation.

The bootstrap aims to be complete while staying as agnostic as possible.
For instance, it doesn't install any browser,
which all too often is a personal choice.

All tasks are grouped into 3 (three) major tags:

-   essential
-   recommended
-   extra

with some minor tags.

You can put your personal tasks in `custom.yaml`,
which will be included in the playbook,
but ignored by git.

##  Tips

-   To list all tags,

    ```shell
    ansible-playbook macos.yaml --list-tags
    ```

    or

    ```shell
    ansible-playbook macos.yaml --list-tags 2>/dev/null | grep TASK | yq '.["TASK TAGS"] | @tsv | sub("\t" ; "\n")'
    ```

-   To show only rules with a specific tag,

    ```shell
    ansible-playbook macos.yaml --list-task --tag essential
    ```

    or

    ```shell
    yq '.[].tasks[] | select(.tags[] | contains("essential"))' macos.yaml
    ```

-   To run only specific tags,

    ```
    ansible-playbook macos.yaml --tags essential,recommended
    ```

-   To skip a specific tag,

    ```
    ansible-playbook macos.yaml --skip-tags extra
    ```

##  Bugs

### Apps Store

Ansible uses `mas` to install software from Apps Store.
But since Ventura,
`mas` has a bug that prevents it from installing nor uninstalling apps,
while still can list and search them.
Thus, all `mas` tasks are currently disabled.

Fortunately, there's only one essential software
that is only available on Apps Store: WireGuard.


### Trackpad

Setting Trackpad from CLI (using `defaults`) currently doesn't work,
and must be done manually from GUI.

