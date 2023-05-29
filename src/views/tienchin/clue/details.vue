<template>
    <div>
        <el-row :gutter="16">
            <el-col :span="18">
                <el-row>
                    <el-card class="box-card">
                        <template #header>
                            <div class="card-header">
                                <span>{{ clue.name }} / {{ clue.clueId }}</span>
                            </div>
                        </template>
                        <div>
                            <el-row>
                                <el-col :span="8">
                                    <div style="font-style: italic; font-weight: bold">线索创建时间</div>
                                    <div style="color:#8392a6">{{ parseTime(clue.createTime) }}</div>
                                </el-col>
                                <el-col :span="8">
                                    <div style="font-style: italic; font-weight: bold">客户手机号码</div>
                                    <div style="color:#8392a6">{{ clue.phone }}</div>
                                </el-col>
                                <el-col :span="8">
                                    <div style="font-style: italic; font-weight: bold">渠道来源</div>
                                    <div style="color:#8392a6">{{ clue.channelName }}</div>
                                </el-col>
                            </el-row>
                            <el-row>
                                <el-col :span="8">
                                    <div style="font-style: italic; font-weight: bold">线索分配者</div>
                                    <div style="color:#8392a6">{{ clue.allocator }}</div>
                                </el-col>
                                <el-col :span="8">
                                    <div style="font-style: italic; font-weight: bold">线索归属时间</div>
                                    <div style="color:#8392a6">{{ clue.belongTime }}</div>
                                </el-col>
                                <el-col :span="8">
                                    <div style="font-style: italic; font-weight: bold">线索归属人</div>
                                    <div style="color:#8392a6">{{ clue.owner }}</div>
                                </el-col>
                            </el-row>
                            <el-row>
                                <el-col :span="16">
                                    <div style="font-style: italic; font-weight: bold">参与活动信息</div>
                                    <div style="color:#8392a6">{{ clue.activityName }} / {{ clue.activityInfo }}</div>
                                </el-col>
                                <el-col :span="8">
                                    <el-button :disabled="isFileDisabledEdit" type="danger" @click="showInvalidateClueDialog">
                                        无效线索
                                    </el-button>
                                    <el-button :disabled="isFileDisabledEdit" type="primary" @click="handleToBusiness">转为商机</el-button>
                                </el-col>
                            </el-row>
                        </div>
                    </el-card>
                </el-row>
                <el-row>
                    <el-card class="box-card">
                        <template #header>
                            <div class="card-header">
                                <span>线索跟进</span>
                            </div>
                        </template>
                        <div>
                            <el-row :gutter="24">
                                <el-col :span="8">
                                    <div style="font-style: italic;font-weight: bold">客户姓名</div>
                                    <div>
                                        <el-input :disabled="isFileDisabledEdit" v-model="clue.name"></el-input>
                                    </div>
                                </el-col>
                                <el-col :span="8">
                                    <div style="font-style: italic;font-weight: bold">性别</div>
                                    <div>
                                        <el-select v-model="clue.gender" placeholder="请选择" :disabled="isFileDisabledEdit">
                                            <el-option
                                                    v-for="dict in sys_user_sex"
                                                    :key="dict.value"
                                                    :label="dict.label"
                                                    :value="parseInt(dict.value)"
                                            ></el-option>
                                        </el-select>
                                    </div>
                                </el-col>
                                <el-col :span="8">
                                    <div style="font-style: italic;font-weight: bold">年龄</div>
                                    <div>
                                        <el-input-number :disabled="isFileDisabledEdit" v-model="clue.age" :min="1"
                                                         :max="100"/>
                                    </div>
                                </el-col>
                            </el-row>
                            <el-row :gutter="36">
                                <el-col :span="8">
                                    <div style="font-style: italic;font-weight: bold">客户微信号码</div>
                                    <div>
                                        <el-input :disabled="isFileDisabledEdit" v-model="clue.wechat"></el-input>
                                    </div>
                                </el-col>
                                <el-col :span="8">
                                    <div style="font-style: italic;font-weight: bold">客户QQ号码</div>
                                    <div>
                                        <el-input :disabled="isFileDisabledEdit" v-model="clue.qq"></el-input>
                                    </div>
                                </el-col>
                                <el-col :span="8">
                                    <div style="font-style: italic;font-weight: bold">客户手机号码</div>
                                    <div>
                                        <el-input :disabled="isFileDisabledEdit" v-model="clue.phone"></el-input>
                                    </div>
                                </el-col>
                            </el-row>
                            <el-row>
                                <el-col :span="24">
                                    <div style="font-style: italic;font-weight: bold">客户意向等级</div>
                                    <div>
                                        <el-radio-group v-model="clue.level" :disabled="isFileDisabledEdit">
                                            <el-radio :label="parseInt(cl.value)" :key="index"
                                                      v-for="(cl,index) in clue_level">{{
                                                    cl.label
                                                }}
                                            </el-radio>
                                        </el-radio-group>
                                    </div>
                                </el-col>
                            </el-row>
                            <el-row>
                                <el-col :span="24">
                                    <div style="font-style: italic;font-weight: bold">客户意向类型</div>
                                    <div>
                                        <el-radio-group v-model="clue.subject" :disabled="isFileDisabledEdit">
                                            <el-radio :label="parseInt(ct.value)" :key="index"
                                                      v-for="(ct,index) in course_type">{{
                                                    ct.label
                                                }}
                                            </el-radio>
                                        </el-radio-group>
                                    </div>
                                </el-col>
                            </el-row>
                            <el-row>
                                <el-col :span="24">
                                    <div style="font-style: italic;font-weight: bold">跟进记录</div>
                                    <div>
                                        <el-input
                                                v-model="clue.record"
                                                :rows="5"
                                                type="textarea"
                                                :disabled="isFileDisabledEdit"
                                                placeholder="请输入内容"
                                        />
                                    </div>
                                </el-col>
                            </el-row>
                            <el-row>
                                <el-col :span="24">
                                    <div style="font-style: italic;font-weight: bold">下次跟进时间</div>
                                    <div>
                                        <el-date-picker
                                                v-model="clue.nextTime"
                                                type="datetime"
                                                :disabled="isFileDisabledEdit"
                                                clearable
                                                value-format="YYYY-MM-DD hh:mm:ss"
                                                format="YYYY-MM-DD hh:mm:ss"
                                                placeholder="请选择下次跟进时间"
                                        />
                                    </div>
                                </el-col>
                            </el-row>
                            <el-row>
                                <el-col>
                                    <div style="display: flex;justify-content: flex-end">
                                        <el-button :disabled="isFileDisabledEdit" type="primary" @click="handleClueFollow">
                                            提交
                                        </el-button>
                                        <el-button @click="goBack">返回</el-button>
                                    </div>
                                </el-col>
                            </el-row>
                        </div>
                    </el-card>
                </el-row>
            </el-col>
            <el-col :span="6">
                <el-card class="box-card">
                    <template #header>
                        <div class="card-header">
                            <span>线索跟进记录</span>
                        </div>
                    </template>
                    <div>
                        <div v-for="(r,index) in records" :key="index" style="margin-bottom: 20px">
                            <div>
                                <span style="font-weight: bold;font-style: italic">{{ r.createBy }} : </span>
                                <span>{{ r.info }}</span>
                                <span style="font-size: 12px;color: #8392a6">&emsp;{{ parseTime(r.createTime) }}</span>
                            </div>
                        </div>
                    </div>
                </el-card>
            </el-col>
        </el-row>
        <!-- 无效线索对话框 -->
        <el-dialog title="无效线索" v-model="invalidateClueDialog" width="500px" append-to-body>
            <el-form ref="invalidateClueRef" :model="invalidateClueForm" :rules="invalidateClueFormRules"
                     label-width="80px">
                <el-row>
                    <el-col :span="24">
                        <el-form-item label="原因" prop="reason_type">
                            <el-select
                                    v-model="invalidateClueForm.reason_type"
                                    placeholder="请选择无效原因"
                                    clearable
                                    style="width: 100%"
                            >
                                <el-option
                                        v-for="ci in clue_invalidate"
                                        :key="ci.value"
                                        :label="ci.label"
                                        :value="ci.label"
                                />
                            </el-select>
                        </el-form-item>
                    </el-col>
                </el-row>
                <el-row>
                    <el-col :span="24">
                        <el-form-item label="备注" prop="reason_remark">
                            <el-input
                                    v-model="invalidateClueForm.reason_remark"
                                    :rows="3"
                                    type="textarea"
                                    placeholder="请输入内容"
                            />
                        </el-form-item>
                    </el-col>
                </el-row>
            </el-form>
            <template #footer>
                <div class="dialog-footer">
                    <el-button type="primary" @click="handleClueInvalidate">确 定</el-button>
                    <el-button @click="cancelAssignClue">取 消</el-button>
                </div>
            </template>
        </el-dialog>
    </div>
</template>

<script setup>
import { getCurrentInstance, onMounted } from "vue";
import { getClueById, clueFollow, getClueRecordByClueId, invalidClue, clueToBusiness } from "@/api/tienchin/clue";
import { parseTime } from "@/utils/channel";
import { ElMessage, ElMessageBox } from "element-plus";

const type = ref("");
const clue = ref({});
const records = ref([])
const invalidateClueDialog = ref(false)
const isFileDisabledEdit = ref(false)

const {proxy} = getCurrentInstance();
const router = useRouter();

const {
    sys_user_sex,
    clue_level,
    course_type,
    clue_invalidate
} = proxy.useDict("sys_user_sex", "clue_level", "course_type", "clue_invalidate");

const data = reactive({
    invalidateClueForm: {},
    invalidateClueFormRules: {
        reason_type: [{required: true, message: "失效原因不能为空", trigger: "blur"}]
    }
});

const {invalidateClueForm, invalidateClueFormRules} = toRefs(data);

function handleClueInvalidate() {
    proxy.$refs["invalidateClueRef"].validate(valid => {
        if (valid) {
            if (invalidateClueForm.value.reason_type != undefined) {
                clue.value.record = invalidateClueForm.value.reason_type + ":" + (invalidateClueForm.value.reason_remark ? invalidateClueForm.value.reason_remark : '');
                invalidClue(clue.value).then(response => {
                    proxy.$modal.msgSuccess("无效线索设置成功");
                    router.go(-1);
                })
            } else {
                invalidClue(clue.value).then(response => {
                    proxy.$modal.msgSuccess("无效线索设置失败");
                    router.go(-1);
                })
            }
        }
    })
}

function showInvalidateClueDialog() {
    invalidateClueDialog.value = true;
}

function getRecords(clueId) {
    getClueRecordByClueId(clueId).then(response => {
        records.value = response.data;
    })
}

function handleClueFollow() {
    clueFollow(clue.value).then(response => {
        router.go(-1);
    })
}

function handleToBusiness() {
    ElMessageBox.confirm(
        '是否要将当前线索转换为客户?',
        '提示',
        {
            confirmButtonText: '确认',
            cancelButtonText: '取消',
            type: 'warning',
        }
    ).then(() => {
        clueToBusiness(clue.value.clueId).then(response => {
            router.go(-1);
        })
    }).catch(() => {
        ElMessage({
            type: 'info',
            message: '取消操作',
        })
    })
}

function goBack() {
    router.go(-1);
}

onMounted(() => {
    const clueId = proxy.$route.params && proxy.$route.params.clueId;
    type.value = proxy.$route.params && proxy.$route.params.type;
    isFileDisabledEdit.value = type.value == 'view';
    handleClue(clueId);
    getRecords(clueId);
})

function handleClue(clueId) {
    getClueById(clueId).then(response => {
        clue.value = response.data;
        isFileDisabledEdit.value = isFileDisabledEdit.value || clue.value.status == 4;
    })
}

</script>

<style scoped>

.box-card {
    margin-top: 15px;
    width: 100%;
}

.el-row {
    margin-bottom: 15px;
}

.el-row:last-child {
    margin-bottom: 10px;
}
</style>