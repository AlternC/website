To add a block on menu, you can use 'hook_menu'
Our plugin is named **example**, then we must have a class 

In /usr/share/alternc/panel/class/m_example.php
    class m_example  {
    }

Display a block with a title linked

   function hook_menu() {
        $obj = array(
            'title' => _("Example"),
            'ico' => 'images/example.png',
            'link' => 'http://www.alternc.net',
            'pos' => 100,
        );
        return $obj;
   }

Display a block with sublink

    function hook_menu() {
        $obj = array(
            'title' => _("Example"),
            'ico' => 'images/example.png',
            'pos' => 100,
            'link' => 'toggle',
            'links' => array(
                array(
                   'txt' => _("First sub item"),
                   'url' => 'example_list.php',
                   'ico' => '',
                   'class' => '',
                )
            )
        );
        return $obj;
    }


