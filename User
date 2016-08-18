<?php

namespace app\models;

use Yii;

/**
 * This is the model class for table "user".
 *
 * @property integer $id
 * @property string $login
 * @property string $password
 * @property string $email
 * @property bolean active
 */
class User extends \yii\db\ActiveRecord implements \yii\web\IdentityInterface
{
    public $authKey;
    public $accessToken;  
    
    /**
     * @inheritdoc
     */
    public static function tableName()
    {
        return 'user';
    }

    /**
     * @inheritdoc
     */
    public function rules()
    {
        return [
            [['login', 'password', 'email'], 'required'],
            [['login', 'password', 'email'], 'string', 'max' => 255],
            [['login'], 'unique'],
            [['email'], 'unique'],
        ];
    }

    /**
     * @inheritdoc
     */
    public function attributeLabels()
    {
        return [
            'id' => Yii::t('app', 'ID'),
            'login' => Yii::t('app', 'Login'),
            'password' => Yii::t('app', 'Password'),
            'email' => Yii::t('app', 'Email'),
        ];
    }
    
    public function save($runValidation = true, $attributeNames = null){
        
        $this->password = Yii::$app->getSecurity()->generatePasswordHash($this->password);
        return parent::save($runValidation, $attributeNames);
    }
    
    
    //funkcje do autoryczacji:
    /**
     * Finds an identity by the given ID.
     *
     * @param string|integer $id the ID to be looked for
     * @return IdentityInterface|null the identity object that matches the given ID.
     */
    public static function findIdentity($id)
    {
        return static::findOne($id);
    }
    
    /**
     * Finds an identity by the given token.
     *
     * @param string $token the token to be looked for
     * @return IdentityInterface|null the identity object that matches the given token.
     */
    public static function findIdentityByAccessToken($token, $type = null)
    {
        return static::findOne(['access_token' => $token]);
    }
    
    public static function findByUsername($login){
		return self::findOne(['login'=>$login]);
	}

    /**
     * @return int|string current user ID
     */
    public function getId()
    {
        return $this->id;
    }

    /**
     * @return string current user auth key
     */
    public function getAuthKey()
    {
        return $this->authKey;
    }

    /**
     * @param string $authKey
     * @return boolean if auth key is valid for current user
     */
    public function validateAuthKey($authKey)
    {
        return $this->getAuthKey() === $authKey;
    }
    
  public function validatePassword($password){
		return Yii::$app->getSecurity()->validatePassword($password, $this->password);
        //$this->password === Yii::$app->getSecurity()->generatePasswordHash($password);
	}
    
    public function beforeSave($insert)
    {
        if (parent::beforeSave($insert)) {
            if ($this->isNewRecord) {
                $this->authKey = \Yii::$app->security->generateRandomString();
            }
            return true;
        }
        return false;
    }
}
