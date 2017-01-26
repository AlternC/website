To add a block on menu, you can use 'hook_menu'
Our plugin is named **example**, then we must have a class 

In /usr/share/alternc/panel/class/m_example.php

Display a block with a title linked

    class m_example  {

        function hook_menu() {
        $obj = array(
            'title' => _("Example"),
            'ico' => 'images/example.png',
            'link' => 'toggle',
            'pos' => 100,
            'links' => 'http://www.alternc.net',
        );
        return $obj;
        }
    }