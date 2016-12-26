#### GEM

1.rubocop 这个gem会帮你自动批改你的代码
gem "rubocop"   然后在terminal输入rubocop即可

2.gem 'pry' 可以强行中断错误的地方并进入rails console
 使用  在错误代码上 输入`binding.pry

#### 3.markdown（pagedown-bootstrap-rails）

##### 如何安装pagedown-bootstrap-rails（markdown）

   1.`gem "pagedown-bootstrap-rails"`

```
`gem "font-awesome-rails" `
```

   2.application.scss 加入 
   `@import "pagedown_bootstrap"`
   `@import "font-awesome"`

   3.application.js 加入

```ruby
    //= require bootstrap-sprockets
    //= require pagedown_bootstrap
    //= require pagedown_init
```

```
4.任意.coffee下加入
```

```ruby
     $ ->
        $('textarea.wmd-input').each (i, input) ->
          attr = $(input).attr('id').split('wmd-input')[1]
          converter = new Markdown.Converter()
          Markdown.Extra.init(converter)
          help =
            handler: () ->
              window.open('http://daringfireball.net/projects/markdown/syntax')
              return false
            title: "<%= I18n.t('components.markdown_editor.help', default: 'Markdown Editing Help') %>"
          editor = new Markdown.Editor(converter, attr, help)
          editor.run()	
```

```
​
```

```
5.在SimpleForm中使用pagedown
```

```ruby
    class PagedownInput < SimpleForm::Inputs::TextInput
      def input
        out = "<div id=\"wmd-button-bar-#{attribute_name}\"></div>#{wmd_input}"

        if input_html_options[:preview]
          out << "<div id=\"wmd-preview-#{attribute_name}\" class=\"wmd-preview\"></div>"
        end

        out.html_safe
      end

      private

      def wmd_input
        @builder.text_area(
          attribute_name,
          input_html_options.merge(
            class: 'wmd-input form-control', id: "wmd-input-#{attribute_name}"
          )
        )
      end
    end
```

```
6.使用
```

```ruby
    <%= f.input :description, as: :pagedown, input_html: { preview: true, rows: 10 } %>
```



#### 4.trix 编辑器的使用

#### https://github.com/maclover7/trix 

想要使用 trix在页面上显示
 `<%= simple_format(@course.description) %>` simple_format会把html语言转化成可视化语言.



#### 5.SEO helper的使用

1.`gem "seo_helper"`

2.

```erb
app/views/layouts/name
#在head里加入
	<head>
        <%= render_page_title_tag %>
        <%= render_page_description_meta_tag %>
    </head>
    
```

3.

```ruby
#在对应的controller里的方法加入
  def index
    @user = current_user
    set_page_title "name"
    set_page_description "name"
  end
```