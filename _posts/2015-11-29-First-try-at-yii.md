---
title: yii的一次简单尝试
layout: post
---

# yii的一次简单尝试

## 声明

本文的许多内容是根据[Yii 2.0 权威指南](http://www.yiichina.com/guide)的内容结合自己的练习过程写成，本文档所有内容遵循[Yii 文档使用许可](http://www.yiiframework.com/doc/terms/)

有关本笔记的任何问题或者指出错误，请联系我


## 目的

*  创建一个用户可以通过表单输入数据的模型
*  使用规则验证表单数据
*  在view中生成这个表单

## 预备知识

一个基本的yii应用文件夹的目录结构如下：

	basic/              应用根目录
    composer.json       Composer 配置文件
    config/             应用配置及其它配置
        console.php     控制台应用配置信息
        web.php         Web 应用配置信息
    commands/           控制台命令类
    controllers/        控制器类
    models/             模型类
    runtime/            Yii 在运行时生成的文件，例如日志和缓存文件
    vendor/             已安装的 Composer 包，包括 Yii 框架自身
    views/              视图文件
    web/                Web 应用根目录，包含 Web 入口文件
        assets/         Yii 发布的资源文件（javascript 和 css）
        index.php       应用入口文件
    yii                 Yii 控制台命令执行脚本

## 创建模型

在basic/models/中创建模型，这里叫EntryForm.php

	<?php

	namespace app\models;

	use yii\base\Model;


	class EntryForm extends Model {
	public $name;
	public $email;
	
	public function rules() {
		return [
			[ [ 'name' , 'email' ] , 'required' ],
			[ 'email' , 'email' ],
		];
	}
	}
	?>

在basic/controllers/SiteController.php的SiteController类中创建entry操作：

	// entry操作，用于模型Entry
	public function actionEntry() {
		$model = new EntryForm;
		if( $model -> load( Yii::$app->request->post() ) && $model->validate() ) {
			// ok and do something
			
			return $this->render( 'entry-confirm' , [ 'model' => $model] );
		} else {
			return $this->render( 'entry' , [ 'model' => $model ] );
		}
	}

不要忘记声明use EntryForm：

	// SiteContrller.php开头
	// ..其它代码
	use app\models\EntryForm;
	// ..其它代码


##创建视图

在basic/views/site中创建entry-confirm和entry两个视图（见刚才创建entry操作时，有两个渲染操作，一个渲染entry-confirm视图，一个渲染entry视图）

entry视图

	// entry.php
    <?php
    use yii\helpers\Html;
    use yii\widgets\ActiveForm;
    ?>
    <?php $form = ActiveForm::begin(); ?>
    
    <?= $form->field($model, 'name') ?>
    
    <?= $form->field($model, 'email') ?>
    
    <div class="form-group">
    <?= Html::submitButton('Submit', ['class' => 'btn btn-primary']) ?>
    </div>
    
    <?php ActiveForm::end(); ?>

entry-confirm视图

    <?php
    	use yii\helpers\Html;
    ?>
    <p>You have entered the following information:</p>
    
    <ul>
    	<li> <label>Name</label>: <?=Html::encode($model->name) ?> </li>
    	<li> <label>Email</label>: <?=Html::encode($model->email) ?></li>
    </ul>

## 完成

模型和视图创建完毕后可以通过在index.php后加入参数?r=site/entry访问，即如果主页是

    http://`hostname/index.php`

则使用

	http://hostname/index.php?r=site/entry