<script>
import IvueBreadcrumbsItem from '../ivue-breadcrumbs-item/ivue-breadcrumbs-item'

const prefixCls = 'ivue-breadcrumbs';

export default {
      name: prefixCls,
      props: {
            items: {
                  type: Array,
                  default: () => {
                        return []
                  }
            },
            /*
            * 分隔符
            * 
            * @type {String}
            */
            divider: {
                  type: String,
                  default: '/'
            },
            /*
            * 中间对齐
            * 
            * @type {Boolean}
            */
            justifyCenter: Boolean,
            /*
            * 尾部对齐
            * 
            * @type {Boolean}
            */
            justifyEnd: Boolean
      },
      computed: {
            // 设置分隔符
            computedDivider () {
                  return this.$slots.divider ? this.$slots.divider : this.divider;
            },
            classes () {
                  return `${prefixCls}`;
            },
            // 设置对齐位置
            styles () {
                  const justify = this.justifyCenter ? 'center' : this.justifyEnd ? 'flex-end' : 'flex-start';

                  return {
                        'justify-content': justify
                  }
            }
      },
      methods: {
            // 渲染子节点添加分隔符
            genChildren () {
                  // 创建节点
                  const h = this.$createElement;

                  const children = [];

                  const dividerData = { staticClass: `${prefixCls}--divider` };

                  if (this.items.length > 0) {

                        this.items.forEach((item, index) => {
                              children.push(h(IvueBreadcrumbsItem, {
                                    props: {
                                          href: item.href,
                                          disabled: item.disabled
                                    }
                              }, [item.text]));


                              if (index !== (this.items.length - 1)) {
                                    children.push(h('span', dividerData, this.computedDivider));
                              }

                        })
                  }
                  else {
                        if (!this.$slots.default) {
                              return null
                        }

                        let createDividers = false;

                        for (let i = 0; i < this.$slots.default.length; i++) {
                              const elm = this.$slots.default[i];

                              if ((!elm.componentOptions || elm.componentOptions.Ctor.options.name !== `ivue-breadcrumbs-item`)) {
                                    children.push(elm);
                              }
                              else {
                                    if (createDividers) {
                                          children.push(h('span', dividerData, this.computedDivider));
                                    }
                                    children.push(elm);

                                    createDividers = true;
                              }
                        }
                  }

                  return children;
            }
      },
      render (h) {
            return h('div', {
                  'class': this.classes,
                  style: this.styles
            }, this.genChildren());
      }
}
</script>

