#ecshop  
##验证码  
首先是生成一个验证码（在后台进行图像处理），记录在session里面，然后提交到后台之后会对相应的session进行验证，从而判断是否正确； 
		<?php
		
		/**
		 * ECSHOP 生成验证码
		 * ============================================================================
		 * * 版权所有 2005-2012 上海商派网络科技有限公司，并保留所有权利。
		 * 网站地址: http://www.ecshop.com；
		 * ----------------------------------------------------------------------------
		 * 这不是一个自由软件！您只能在不用于商业目的的前提下对程序代码进行修改和
		 * 使用；不允许对程序代码以任何形式任何目的的再发布。
		 * ============================================================================
		 * $Author: liubo $
		 * $Id: captcha.php 17217 2011-01-19 06:29:08Z liubo $
		*/
		
		define('IN_ECS', true);
		define('INIT_NO_SMARTY', true);
		
		require(dirname(__FILE__) . '/includes/init.php');
		require(ROOT_PATH . 'includes/cls_captcha.php');
		
		//yyy修改start
		$img = new captcha('../data/captcha/', $_CFG['captcha_width'], $_CFG['captcha_height']);
		//yyy修改end
		@ob_end_clean(); //清除之前出现的多余输入
		if (isset($_REQUEST['is_login']))
		{
		    $img->session_word = 'captcha_login';
		}
		$img->generate_image();
		
		?>  
生成验证码 
 
    /**
     * 生成图片并输出到浏览器
     *
     * @access  public
     * @param   string  $word   验证码
     * @return  mix
     */
    function generate_image($word = false)
    {
        if (!$word)
        {
            $word = $this->generate_word();
        }

        /* 记录验证码到session */
        $this->record_word($word);//记录到session中

        /* 验证码长度 */
        $letters = strlen($word);

        /* 选择一个随机的方案 */
        mt_srand((double) microtime() * 1000000);

        if (function_exists('imagecreatefromjpeg') && ((imagetypes() & IMG_JPG) > 0))
        {
            $theme  = $this->themes_jpg[mt_rand(1, count($this->themes_jpg))];
        }
        else
        {
            $theme  = $this->themes_gif[mt_rand(1, count($this->themes_gif))];
        }

        if (!file_exists($this->folder . $theme[0]))
        {
            return false;
        }
        else
        {
            $img_bg    = (function_exists('imagecreatefromjpeg') && ((imagetypes() & IMG_JPG) > 0)) ?
                            imagecreatefromjpeg($this->folder . $theme[0]) : imagecreatefromgif($this->folder . $theme[0]);
            $bg_width  = imagesx($img_bg);
            $bg_height = imagesy($img_bg);

            $img_org   = ((function_exists('imagecreatetruecolor')) && PHP_VERSION >= '4.3') ?
                          imagecreatetruecolor($this->width, $this->height) : imagecreate($this->width, $this->height);

            /* 将背景图象复制原始图象并调整大小 */
            if (function_exists('imagecopyresampled') && PHP_VERSION >= '4.3') // GD 2.x
            {
                imagecopyresampled($img_org, $img_bg, 0, 0, 0, 0, $this->width, $this->height, $bg_width, $bg_height);
            }
            else // GD 1.x
            {
                imagecopyresized($img_org, $img_bg, 0, 0, 0, 0, $this->width, $this->height, $bg_width, $bg_height);
            }
            imagedestroy($img_bg);

            $clr = imagecolorallocate($img_org, $theme[1], $theme[2], $theme[3]);

            /* 绘制边框 */
            //imagerectangle($img_org, 0, 0, $this->width - 1, $this->height - 1, $clr);

            /* 获得验证码的高度和宽度 */
            $x = ($this->width - (imagefontwidth(5) * $letters)) / 2;
            $y = ($this->height - imagefontheight(5)) / 2;
            imagestring($img_org, 5, $x, $y, $word, $clr);

            header('Expires: Thu, 01 Jan 1970 00:00:00 GMT');

            // HTTP/1.1
            header('Cache-Control: private, no-store, no-cache, must-revalidate');
            header('Cache-Control: post-check=0, pre-check=0, max-age=0', false);

            // HTTP/1.0
            header('Pragma: no-cache');
            if ($this->img_type == 'jpeg' && function_exists('imagecreatefromjpeg'))
            {
                header('Content-type: image/jpeg');
                imageinterlace($img_org, 1);
                imagejpeg($img_org, false, 95);
            }
            else
            {
                header('Content-type: image/png');
                imagepng($img_org);
            }

            imagedestroy($img_org);

            return true;
        }
    }

验证验证码  

    /**
     * 检查给出的验证码是否和session中的一致
     *
     * @access  public
     * @param   string  $word   验证码
     * @return  bool
     */
    function check_word($word)
    {
        $recorded = isset($_SESSION[$this->session_word]) ? base64_decode($_SESSION[$this->session_word]) : '';
        $given    = $this->encrypts_word(strtoupper($word));

        return (preg_match("/$given/", $recorded));
    }




#后台显示原理
后台分为三个frame，分别为：
	
	  <frameset cols="195, 12, *" framespacing="0" border="0" id="frame-body">
	    <frame src="index.php?act=menu" id="menu-frame" name="menu_frame" frameborder="no" scrolling="yes">
	    <frame src="index.php?act=drag" id="drag-frame" name="drag_frame" frameborder="no" scrolling="no">
	    <frame src="index.php?act=main" id="main-frame" name="main_frame" frameborder="no" scrolling="yes">
	  </frameset>
其中的main_frame就是显示内容的部分，而menu_frame就是菜单的部分；
而菜单的那些链接，通常都是把target设置为main_frame，即在main_frame中打开；
如：  

	<a href="{$child.action}" target="main_frame">{$child.label}</a>