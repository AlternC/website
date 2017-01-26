To add a block on menu, you can use 'hook_menu'
Our plugin is named **example**, then we must have a class 

In /var/class/m_exemple.php

    class m_sample  {

        function hook_menu() {
            $obj = array(
            );
        }
    }